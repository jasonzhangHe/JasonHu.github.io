## 1.`Vue3`简介

- 2020年9月18日，`Vue.js`发布3.0版本，代号：One Piece（海贼王）
- 耗时2年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99位贡献者](https://github.com/vuejs/vue-next/graphs/contributors) 
- github`上的tags地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0

### 1.2 Vue3带来了什么

- 性能的提升

- 打包大小减少41%

- 初次渲染快55%, 更新渲染快133%

- 内存减少54%

  ......

### 1.2.源码的升级

- 使用Proxy代替defineProperty实现响应式

- 重写虚拟DOM的实现和Tree-Shaking

  ......

### 1.3.拥抱TypeScript

- Vue3可以更好的支持TypeScript

### 1.4.新的特性

1. Composition API（组合API）

   - setup配置
   - ref与reactive
   - watch与watchEffect
   - provide与inject
   - ......
2. 新的内置组件
   - Fragment 
   - Teleport
   - Suspense
3. 其他改变

   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除keyCode支持作为 v-on 的修饰符
   - ......

## 2.创建`Vue`工程

###    2.1使用 `vue-cli`创建

```shell
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

### 2.2. 启动项目:

   ```shell
   cd <your-project-name>
   npm install
   npm run dev
   ```

   ### 2.3  使用 `vite `创建

```shell
##   创建过程
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes  # 是否添加ts
✔ Add JSX Support? … No / Yes # 是否添加jsx
✔ Add Vue Router for Single Page Application development? … No / Yes  # 是否添加路由
✔ Add Pinia for state management? … No / Yes  # 是否添加 状态管理库,与vuex类似
✔ Add Vitest for Unit testing? … No / Yes   #  # 是否添加单元测试
✔ Add ESLint for code quality? … No / Yes  # 是否添加语法检测
```

###  2.4  什么是`vite`？—— 新一代前端构建工具。

- 官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

- `vite`官网：https://vitejs.cn

- 优势如下：

  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（`HMR`）。
  - 真正的按需编译，不再等待整个应用编译完成。

- 传统构建 与 `vite`构建对比图

  ![2024020502](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020502.png)

- 传统构建
- ![2024020503](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020503.png)

###  2.5  文件结构 

- `index.htrml`: 程序运行入口文件

  - 在 body 体中只有一个 div 标签，其 id 为 app，这个 id 将会连接到 src/main.js 内容。
    在浏览器打开的瞬间，浏览器中正文部分会瞬间显示 index.html 中定义的正文部分。上面有一个 id 为 app 的挂载点，之后我们的 Vue 根实例就会挂载到该挂载点上

  ![image-20240205212848522](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020505.png)

- `main.js`:    程序入口文件

  -  主要作用是初始化 vue 实例，并引入所需要的插件。新建了一个 vue 实例，并使用 el：#app 链接到 index.html 中的 app，并使用 template 引入组件    和路由相关的内容，也就是说通过 main.js 我们关联到 App.vue 组件

 ![2024020501](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020501.png)

- `env.d.ts`: 热加载入口

  ![image-20240205212916082](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020506.png)

 - node_modules: 依赖库

 - assets:  静态文件: 该目录下适合存放项目中所必须的图标,属于代码的一部分

 - component: 组件文件

 - public :  静态文件: 该目录下项目需要使用的静态资源，有更新动态显示的需求

   -  区别:

     - `public`目录下：使用 / 开头的绝对路径访问

       | 若：/public/images/a.jpg访问： <img src="/images/a.jpg">     |
       | ------------------------------------------------------------ |
       | 若：/public/images/a/b/c/d.jpg访问： <img src="/images/a/b/c/d.jpg" |
     - `src/assets`目录下：使用相对路径访
     
       | 若： /src/assets/a.jpg访问：<img src="../assets/a.jpg">         <img src="@/assets/a.jpg">  @代表src目录 |
       | ------------------------------------------------------------ |
     
       检查元素发现，asset下的图片访问路径被篡改成了： /img/001.923456aefr4trwt44.png
     
       原因是，`webpack`在编译打包过程中，将会把`src`下的所有引用的资源当做模块进行处理。图片也不例外。脚手架默认会将图片直接打包到输出项目目录下的`/img`文件夹中，为了防止命名冲突，还为每张图片重命名。最后将源代码中的`src`路径进行了篡改。使得页面可以正常显示该图片。
     
       再次`webpack`打包时会对这些占用空间足够小的小图标进行优化。将会把一些小图标直接转成`base64`图片编码，为`img`的`src`直接赋值。降低浏览器访问小图片的频率。优化网页访问性能。
     
       #### `assets`下的动态路径的设置方式：
     
       ```html
       <img :src="`../assets/a.jpg`">
       ```
     
       一旦`img`的`src`是动态属性，那么`webpack`将不会对该路径进行篡改，会原封不动的编译该标签。这样，浏览器在加载图片时，就会去找`assets`，将会找不到该图片。
     
       应该如下设置`assets`下的动态路径：
     
       ```html
       <img :src="require('../assets/a.jpg')">
       ```
     
       将该路径经过`require`处理后，会把图片当做模块重命名后放到项目的`/img/`文件夹下，并且将`src`修改为可以访问到的`/img/xxx.xx.jpg`文件路径。正常显示该图片。
     
     - `App.vue`: vue 主组件
     
   
### 2.6  总结 

- `Vite` 项目中,`inex.html`  是项目的入口文件,在项目最外层
- 加载`index.html`  后,`Vite` 解析<script type= "module" src="xxxx"> 指向`JavScript`
- `Vue3`中是通过`createApp` 函数创建一个应用实例



## 3. Vue3 核心语法

### 3.1  Options Api 的弊端 和  Composition Api 的优势

- `Options`类型的`API`,数据、方法、计算属性,是分散在: `data` 、`methods`、`computed`中的,若想新增或者修改一个需求,就需要分别修改:`data` 、`methods`、`computed`,不便于维护和复用

<div style="width:600px;height:370px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f84e4e2c02424d9a99862ade0a2e4114~tplv-k3u1fbpfcp-watermark.image" style="width:600px;float:left" />
</div>
<div style="width:300px;height:370px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5ac7e20d1784887a826f6360768a368~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;width:560px;left" /> 
</div>



















-  Composition Api 的优势

​     可以用函数的方式,更加优雅的组织代码,让相关功能的代码更加有序的组织在一起

<div style="width:500px;height:340px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc0be8211fc54b6c941c036791ba4efe~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>
<div style="width:430px;height:340px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cc55165c0e34069a75fe36f8712eb80~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>

















### 3.2 拉开序幕的setup

- 理解：Vue3.0中一个新的配置项，值为一个函数。

- setup是所有<strong style="color:#DD5145">Composition API（组合API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。

- 组件中所用到的：数据、方法等等，均要配置在setup中。

- setup函数的两种返回值：若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）

-  <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span> 

  - 注意点：

    - 尽量不要与Vue2.x配置混用

    - Vue2.x配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup中的属性、方法。
    - 但在setup中<strong style="color:#DD5145">不能访问到</strong>Vue2.x配置（data、methos、computed...）。
    - 如果有重名, setup优先。

    - setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）

### 3.3  代码

-  vue3 常规方式

```vue
<template>
    <div class="perion">
        <h2>姓名: {{ name }}</h2>
        <h2>年龄: {{ age }} </h2>
        <h2>电话: {{ tel }}</h2>
        <button @click="changeName">点击名称</button>
        <button @click="showAge">点击年龄</button>
        <button @click="showTel">点击电话</button>
    </div>
</template>


<!--  声明使用ts -->
<script lang="ts">
export default {
    name: 'Persion',
    setup() { 
        /// setup中 this  是undefined,vue3 中已经弱化this了
          // console.log(this);
        //  不是响应式  变量
        let name =  "'张三'"
        let age =  '18'
        let tel = '1348390247297490'


        // 这种修改方式页面无反应,值修改
        function changeName() {
            name = 'zs'
            console.log(name);
        }
         function showAge() {
            tel = '121'
            console.log(tel);
        }
        function showTel() {
            tel = '121'
            console.log(tel);
        }
        //  将数据返回,模版才能应用
        return {name,age,tel, changeName, showAge, showTel }
        //  return ()=> '箭头函数'
    },
   
}
</script>
  
<!--   设置样式 -->
<style>
.perion {
    background-color: #ddd;
    box-shadow: 0 0 10px;
    border-radius: 10px;
    padding: 20px;
}
</style>
```
- 使用语法糖

```js
  <template>
      <div class="perion">
          <h2>姓名: {{ name }}</h2>
          <h2>年龄: {{ age }} </h2>
          <h2>电话: {{ tel }}</h2>
          <button @click="changeName">点击名称</button>
          <button @click="showAge">点击年龄</button>
          <button @click="showTel">点击电话</button>
      </div>
  </template>
  
  
  <!--  声明使用ts -->
  <script lang="ts">
  export default { 
      name: 'Persion'
  }
  </script>
  
  <script lang="ts" setup >
       let name = "'张三'"
       let age = '18'
       let tel = '1348390247297490'
         // 这种修改方式页面无反应,值修改
  function changeName() {
      name = 'zs'
      console.log(name);
  }
  function showAge() {
      tel = '121'
      console.log(tel);
  }
  function showTel() {
      tel = '121'
      console.log(tel);
  }
  
  </script>
    
  <!--   设置样式 -->
  <style>
  .perion {
      background-color: #ddd;
      box-shadow: 0 0 10px;
      border-radius: 10px;
      padding: 20px;
  }
  </style>
```

  扩展:  上述代码.还需要有一个不写`setUp`的`script`标签,去指定组件名字,比较麻烦,我们可以借助`vite`中的插件简化

   1. `npm i  vite-plugin-vue-setup-extend   -D`

   2. `vue.config.ts`

      ```js
      import  VueSetUpExtend from 'vite-plugin-vue-setup-extend'
      
      export default defineConfig({
        plugins: [
          [VueSetUpExtend()]
        ]
      })
      ```

​    ![image-20240206163518123](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020601.png)

 3. 数据属性与分析

    ![image-20240206165023723](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020602.png)



###  3.2.ref函数

- 作用: 定义一个响应式的数据

- 语法: ```const xxx = ref(initValue)``` 

  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference对象，简称ref对象）</strong>。
  - JS中操作数据： ```xxx.value```
  - 模板中读取数据: 不需要.value，直接：```<div>{{xxx}}</div>```

  ​       ![image-20240206170507419](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020603.png)



- 备注：

  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠``Object.defineProperty()``的```get```与```set```完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了Vue3.0中的一个新函数—— ```reactive```函数。

  ​       ![image-20240206170538353](C:\Users\jasonzhang\Desktop\JasonHu-master\docs\vue\VUE创建应用程序.assets\2024020604.png)

### 3.3.reactive函数

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用```ref```函数）
- 语法：```const 代理对象= reactive(源对象)```接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy的实例对象，简称proxy对象）</strong>
- reactive定义的响应式数据是“深层次的”。
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

### 3.4.Vue3.0中的响应式原理

- vue2.x的响应式

- 实现原理：

  - 对象类型：通过```Object.defineProperty()```对属性的读取、修改进行拦截（数据劫持）。

  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。

    ```js
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```

- 存在问题：

  - 新增属性、删除属性, 界面不会更新。
  - 直接通过下标修改数组, 界面不会自动更新。

### 3.5.Vue3.0的响应式

- 实现原理: 

  - 通过Proxy（代理）:  拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。

  - 通过Reflect（反射）:  对源对象的属性进行操作。

  - MDN文档中描述的Proxy与Reflect：

    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      ```js
      new Proxy(data, {
      	// 拦截读取属性值
          get (target, prop) {
          	return Reflect.get(target, prop)
          },
          // 拦截设置属性值或添加新属性
          set (target, prop, value) {
          	return Reflect.set(target, prop, value)
          },
          // 拦截删除属性
          deleteProperty (target, prop) {
          	return Reflect.deleteProperty(target, prop)
          }
      })
      
      proxy.name = 'tom'   
      ```

### 3.6.reactive对比ref

-  从定义数据角度对比：
   -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
   -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
   -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。
   -  ref 底层实现: reactive
-  从原理角度对比：
   -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
   -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。
-  从使用角度对比：
   -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
   -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

### 3.7.setup的两个注意点

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。

- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。

### 3.8. 区别

- `ref`创建的变量必须使用`.value`(可以使用`volar` 插件自动添加`.value`)

  ![image-20240206193521455](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020605.png)

- `reactive` 重新分配一个新对象,会**失去**响应式(可以使用`Object.assign`)

   ![image-20240206194126301](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020606.png)

### 3.9.使用原则

- 若需要一个基本类型的响应式数据 必须使用`ref`
- 若需要一个响应式对象,层级不深,`ref`,`reactive`都可以
- 若需要一个响应式对象,且层级较深,推荐使用`reactive`

### 3.10. toRefs 与 toRef 

- 作用：将一个响应式对象(```reactive```)中的每一个属性,转换为`ref`对象
- 备注:  `toRefs` 与 `toRef` 功能一致,但`toRefs`可以批量转换
- `toRef`语法：```const name = toRef(person,'name')```
- `toRefs`语法: ```const {name,age}= toRefs(person)```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```

### 3.11. 计算属性与监视

 computed函数

- 与Vue2.x中computed配置功能一致

- 写法

  ```js
  import {computed} from 'vue'
  
  setup(){
      ...
  	//计算属性——简写 只读
      let fullName = computed(()=>{
          return person.firstName + '-' + person.lastName
      })
      //计算属性——完整  可读可写
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
  }
  ```

  注： 方法没有缓存,多次调用多次重新计算.计算属性有缓存,多次调用检测无数据变化则只计算一次

### 3.12.watch函数

- 与Vue2.x中watch配置功能一致

- 两个小“坑”：

  - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
  - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

  ```js
  //情况一：监视ref定义的响应式数据
  watch(sum,(newValue,oldValue)=>{
  	console.log('sum变化了',newValue,oldValue)
  },{immediate:true})
  
  //情况二：监视多个ref定义的响应式数据
  watch([sum,msg],(newValue,oldValue)=>{
  	console.log('sum或msg变化了',newValue,oldValue)
  }) 
  
  /* 情况三：监视reactive定义的响应式数据
  			若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
  			若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
  */
  watch(person,(newValue,oldValue)=>{
  	console.log('person变化了',newValue,oldValue)
  },{immediate:true,deep:false}) //此处的deep配置不再奏效
  
  //情况四：监视reactive定义的响应式数据中的某个属性
  watch(()=>person.job,(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true}) 
  
  //情况五：监视reactive定义的响应式数据中的某些属性
  watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true})
  
  //特殊情况
  watch(()=>person.job,(newValue,oldValue)=>{
      console.log('person的job变化了',newValue,oldValue)
  },{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
  ```
### 3.13.watchEffect函数

  - watch的套路是：既要指明监视的属性，也要指明监视的回调。

  - watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

  - watchEffect有点像computed：

    - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
    - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(()=>{
      const x1 = sum.value
      const x2 = person.age
      console.log('watchEffect配置的回调执行了')
  })
  ```
### 3.14 .[生命周期](https://www.cnblogs.com/Yan3399/p/17587182.html)

<div style="border:1px solid black;width:520px;float:left;margin-right:20px;"><strong>vue2.x的生命周期</strong><img src="https://v2.cn.vuejs.org/images/lifecycle.png" alt="lifecycle_2" style="zoom:33%;width:2500px" /></div><div style="border:1px solid black;width:520px;height:1330px;float:left"><strong>vue3.0的生命周期</strong><img src="https://cn.vuejs.org/assets/lifecycle.DLmSwRQE.png" alt="lifecycle_2" style="zoom:33%;width:2500px" /></div>





































































Vue2.x中钩子对应关系如下：

- `beforeCreate`===>`setup()`
- `created`=======>`setup()`
- `beforeMount` ===>`onBeforeMount`
- `mounted`=======>`onMounted`
- `beforeUpdate`===>`onBeforeUpdate`
- `updated` =======>`onUpdated`
- `beforeUnmount` ==>`onBeforeUnmount`
- `unmounted` =====>`onUnmounted`

### 3.15.自定义hook函数

- 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。

- 类似于vue2.x中的mixin。

- 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。

### 3.16.toRef

- 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
- 语法：```const name = toRef(person,'name')```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)

## 4.其它 Composition API

### 4.1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

### 4.2.readonly 与 shallowReadonly

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

### 4.3.toRaw 与 markRaw

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

### 4.4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

```js
<template>
	<input type="text" v-model="keyword">
	<h3>{{keyword}}</h3>
</template>

<script>
	import {ref,customRef} from 'vue'
	export default {
		name:'Demo',
		setup(){
			// let keyword = ref('hello') //使用Vue准备好的内置ref
			//自定义一个myRef
			function myRef(value,delay){
				let timer
				//通过customRef去实现自定义
				return customRef((track,trigger)=>{
					return{
						get(){
							track() //告诉Vue这个value值是需要被“追踪”的
							return value
						},
						set(newValue){
							clearTimeout(timer)
							timer = setTimeout(()=>{
								value = newValue
								trigger() //告诉Vue去更新界面
							},delay)
						}
					}
				})
			}
			let keyword = myRef('hello',500) //使用程序员自定义的ref
			return {
				keyword
			}
		}
	}
</script>
```

### 4.5.provide 与 inject

<img src="https://vuejs.org/assets/prop-drilling.FyV2vFBP.png" style="width:300px" />

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

```js
setup(){
	......
    let car = reactive({name:'奔驰',price:'40万'})
    provide('car',car)
    ......
}
```

​             2.后代组件中：

```js
setup(props,context){
	......
    const car = inject('car')
    return {car}
	......
}
```

### 4.6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

## 5.新的组件

### 5.1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

### 5.2.Teleport

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。

  ```vue
  <teleport to="移动位置">
  	<div v-if="isShow" class="mask">
  		<div class="dialog">
  			<h3>我是一个弹窗</h3>
  			<button @click="isShow = false">关闭弹窗</button>
  		</div>
  	</div>
  </teleport>
  ```

### 5.3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```

## 6.其他

### 6.1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。

  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |

### 6.2.其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......

##  7.问题

- `vite`无法启动项目,报以下错误
  
  - ![image-20240205204950367](F:\study\学习笔记仓库\JasonHu-master\docs\vue\images\2024020504.png)
  
  - 解决:  
  
    - node版本低,升级版本,因此想到node版本管理(`nvm`) 
  
    - `nvm` 使用说明(
  
      [摘录网上链接(`ctrl`+鼠标右键)]: nvm使用.md
  
      )
  
      
      

## 8.面试题 

- SetUp 与data、methos、computed 能不能同时存在,是什么执行顺序
  -  都可以同时存在
  - SetUp 不能访问到 data方法里面的 属性,data方法可以获取到SetUp方法属性 
