###  Eureka 注册机制源码解析

![image-20221205162328762](.\images\image-20221205162328762.png)

```Java
// 记录当前InstanceInfo在Server端被修改的时间戳
@Auto
private volatile Long lastUpdatedTimestamp;  
// 记录当前InstanceInfo在Client端被修改的时间戳
@Auto
private volatile Long lastDirtyTimestamp;
// 记录当前Client在Server端的状态 
private volatile InstanceStatus status = InstanceStatus.UP;
// 记录当前Client在Server端的状态status(在Clinet提交注册请求与Renew续约请求时)
private volatile InstanceStatus overriddenStatus = InstanceStatus.UNKNOWN;

```

```java
public enum InstanceStatus {
    UP, // Ready to receive traffic
    DOWN, // Do not send traffic- healthcheck callback failed
    STARTING, // Just about starting- initializations to be done - do not
    // send traffic
    OUT_OF_SERVICE, // Intentionally shutdown for traffic
    UNKNOWN;

    public static InstanceStatus toEnum(String s) {
        if (s != null) {
            try {
                return InstanceStatus.valueOf(s.toUpperCase());
            } catch (IllegalArgumentException e) {
                // ignore and fall through to unknown
                logger.debug("illegal argument supplied to InstanceStatus.valueOf: {}, defaulting to {}", s, UNKNOWN);
            }
        }
        return UNKNOWN;
    }
}
```

```java
// 重写equals  方法  

@Override
public boolean equals(Object obj) {
    if (this == obj) {
        return true;
    }
    if (obj == null) {
        return false;
    }
    if (getClass() != obj.getClass()) {
        return false;
    }
    InstanceInfo other = (InstanceInfo) obj;
    String id = getId();
    if (id == null) {
        if (other.getId() != null) {
            return false;
        }
    } else if (!id.equals(other.getId())) {
        return false;
    }
    return true;
}
```

![image-20221206095457992](.\images\image-20221206095457992.png)

```java
// 保存着所有IndatnceInfo信息
private final Set<InstanceInfo> instances;

private final AtomicReference<List<InstanceInfo>> shuffledInstances;

private final Map<String, InstanceInfo> instancesMap;
```

jersey 框架



### 注册机制

   1. 导入pom

      ```xml
      <dependency>
          <groupId>com.alibaba.cloud</groupId>
          <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
      </dependency>
      ```

 2. 定位jar包

![image-20221206102848955](.\images\image-20221206102848955.png)

3. 查找自动注入方法

![image-20221206103743637](.\images\image-20221206103743637.png)

![image-20221206104734299](.\images\image-20221206104734299.png)

![image-20221206105043329](.\images\image-20221206105043329.png)

4. 源代码 （AutoConfigurationImportSelector，SpringFactoriesLoader）

   ```java
   AutoConfigurationImportSelector
     
   	public String[] selectImports(AnnotationMetadata annotationMetadata) {
   		if (!isEnabled(annotationMetadata)) {
   			return NO_IMPORTS;
   		}
   		AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
   				.loadMetadata(this.beanClassLoader);
   		AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(autoConfigurationMetadata,
   				annotationMetadata);
   		return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
   	}
   
   	/**
   	  *    根据导入的@Configuration类的AnnotationMetadata返回AutoConfigurationImportSelector.AutoConfigurationEntry 。
   	  */
   	protected AutoConfigurationEntry getAutoConfigurationEntry(AutoConfigurationMetadata autoConfigurationMetadata,
   			AnnotationMetadata annotationMetadata) {
   		if (!isEnabled(annotationMetadata)) {
   			return EMPTY_ENTRY;
   		}
   		AnnotationAttributes attributes = getAttributes(annotationMetadata);
   		List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
   		configurations = removeDuplicates(configurations);
   		Set<String> exclusions = getExclusions(annotationMetadata, attributes);
   		checkExcludedClasses(configurations, exclusions);
   		configurations.removeAll(exclusions);
   		configurations = filter(configurations, autoConfigurationMetadata);
   		fireAutoConfigurationImportEvents(configurations, exclusions);
   		return new AutoConfigurationEntry(configurations, exclusions);
   	}
   
   	/**
   	 * 返回应考虑的自动配置类名称。默认情况下，此方法将使用SpringFactoriesLoader和getSpringFactoriesLoaderFactoryClass()加载候选人。
   	 * 参数：metadata ——源元数据 attributes – annotation attributes 
   	 */
   	protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
   		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
   				getBeanClassLoader());
   		Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
   				+ "are using a custom packaging, make sure that file is correct.");
   		return configurations;
   	}
   
   SpringFactoriesLoader
       
       public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
       
         /**
           *  使用给定的类加载器从"META-INF/spring.factories"加载给定类型的工厂实现的完全限定类名。
   		*  参数： factoryType表示工厂的接口或抽象类 classLoader – 用于加载资源的 ClassLoader；可以为null以使用默认值
   		*  异常： IllegalArgumentException – 如果在加载工厂名称时发生错误
           */
       public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
   		String factoryTypeName = factoryType.getName();
   		return loadSpringFactories(classLoader).getOrDefault(factoryTypeName, Collections.emptyList());
   	}
   
   	private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
   		MultiValueMap<String, String> result = cache.get(classLoader);
   		if (result != null) {
   			return result;
   		}
   
   		try {
   			Enumeration<URL> urls = (classLoader != null ?
   					classLoader.getResources(FACTORIES_RESOURCE_LOCATION) :
   					ClassLoader.getSystemResources(FACTORIES_RESOURCE_LOCATION));
   			result = new LinkedMultiValueMap<>();
   			while (urls.hasMoreElements()) {
   				URL url = urls.nextElement();
   				UrlResource resource = new UrlResource(url);
   				Properties properties = PropertiesLoaderUtils.loadProperties(resource);
   				for (Map.Entry<?, ?> entry : properties.entrySet()) {
   					String factoryTypeName = ((String) entry.getKey()).trim();
   					for (String factoryImplementationName : StringUtils.commaDelimitedListToStringArray((String) entry.getValue())) {
   						result.add(factoryTypeName, factoryImplementationName.trim());
   					}
   				}
   			}
   			cache.put(classLoader, result);
   			return result;
   		}
   		catch (IOException ex) {
   			throw new IllegalArgumentException("Unable to load factories from location [" +
   					FACTORIES_RESOURCE_LOCATION + "]", ex);
   		}
   	}
   
   
   
   
   ```

![image-20221206110909931](.\images\image-20221206110909931.png)

![image-20221206111851650](.\images\image-20221206111851650.png)

![image-20221206112014398](.\images\image-20221206112014398.png)





```java
// 搜索策略

public enum SearchStrategy {

   /**
    * Search only the current context.  仅搜索当前上下文
    */
   CURRENT,

   /**
    * Search all ancestors, but not the current context.  搜索所有祖先，但不搜索当前上下文。
    */
   ANCESTORS,

   /**
    * Search the entire hierarchy. 搜索整个层次结构，@ConditionOnBean中默认是ALL。
    */
   ALL

}
```

[RefreshScope](https://blog.csdn.net/JokerLJG/article/details/120254643)

![image-20221207095439385](.\images\image-20221207095439385.png)