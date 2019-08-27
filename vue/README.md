* 对设计模式的理解
* Vue 和 React
* 对 vue 生命周期的理解
* 父子组件间的通信
* Vue指令及用法
* computed和watch和methods的使用和区别
* 双向数据绑定
* keep-alive
* Vuex
* Vue-router
* VUE1 | VUE2 | VUE3 的区别

## 对设计模式的理解
### 什么是MVC(Model-View-Controller)
Model(模型):数据层，负责存储数据  
View(视图):展现层，用户所看到的页面  
Controller(控制器):协调层，负责协调Model和View，根据用户在View上的动作在Model上作出对应的更改，同时将更改的信息返回到View上  

三者之间的关系:  
Controller可以直接访问Model，也可以直接控制View,但是Model和View不能相互通信，相当于COntroller就是介于这两者之间的协调者。

### 什么是MVVM(Model-View-ViewModel)
Model 层代表数据模型，可以在 Model 中定义数据修改和操作的业务逻辑  
View 代表UI组件，它负责将数据模型转化成 UI 展现出来  
ViewModel 是一个同步 View 和 Model 的对象  

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。  

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

### MVC和MVVM的区别
mvc配合过程中我们忽略了一个很重要的操作：数据解析。因为以前的数据都比较简单，现在手机App功能越来越复杂，数据结构也越来越复杂，所以数据解析也就没那么简单了，如果将数据解析的部分放到了Controller里面，那么Controller就将变得相当臃肿，所以聪明的开发者们就专门为数据解析创建出了一个新的类：ViewModel。这就是MVVM的诞生。  
  
mvc 和 mvvm 区别并不大，主要就是 mvc 中 Controller 演变成 mvvm 中的 viewModel。mvvm 主要解决了 mvc 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验，以及当 Model 频繁发生变化，开发者需要主动更新到 View 的问题；mvvm 中开发者只需对数据进行操作就可以。  


## Vue 和 React
### 相似之处：
* 都使用 Virtual DOM。
* 都鼓励组件化应用，本质上就是将应用拆分成一个个功能明确的模块，模块之间再进行相互通信。
* react和vue都有props的概念，允许父子组件通过props进行传值。
* react和vue都有自己的构建工具、状态管理、UI、路由等工具。

### 不同之处:
* vue鼓励你去写近似常见的HTML模板，写起来很接近标准的HTML，只是多了一些属性；react推荐你所有的模板Javascript的语法扩展JSX语法；但是需要注意的是vue在技术上也是支持Render函数和JSX，只是不是默认的而已。
* vue中state不是必须的，数据由data属性在vue对象中进行管理，data就是应用中数据的保存者，可以直接修改；react中的state在应用中是不可变的，意味着它不能直接被修改，需要通过SetState方法进行更新。
* vue应用广泛，目前很火的框架，关注的人很多，2014年2月正式发布；react社区庞大，目前很流行的框架，背后有FaceBook撑腰，发布的时间比vue更早，2013年3月发布，在生产方面经过了很好的测试。
* React Native能在手机上创建原生应用，React在这方面处于领先位置。使用JavaScript, CSS和HTML创建原生移动应用，这是一个重要的革新。Vue社区与阿里合作开发Vue版的React Native——Weex也很不错，但还需经历时间的验证吧。

## 对 vue 生命周期的理解
Vue 实例从创建到销毁的过程，就是生命周期。  
也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期，总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后。  

* beforeCreate  实例创建前：这个阶段实例的data、method是读不到的。
* created  实例创建后：这个节点已经完成了数据观测，属性和方法的运算，watch/event事件回调。mount挂载阶段还没开始，$el属性目前不可见，数据并没有在DOM元素上进行渲染。
> 应用：进行ajax请求异步数据的获取、初始化数据。
* beforeMount  挂载开始之前：相关的render函数首次被调用。
* mounted  挂载之后：el选项的DOM节点被新创建的vm.$el替换，并挂载到实例上去之后调用此生命周期函数。此时实例的数据在DOM节点上进行渲染。
>应用：挂载元素内DOM结点的获取。
* beforeUpdate  更新之前：数据更新时调用，但不进行DOM重新渲染，在数据更新时DOM没渲染前可以在这个生命周期函数里进行状态处理。
* updated  更新之后：这个状态下数据更新并且DOM重新渲染，当这个生命周期函数被调用时，组件DOM已经更新，所以你现在可以执行依赖于DOM的操作。当时李每次进行数据更新时updated都会执行。
> 应用：任何数据的更新，如果要做统一的业务逻辑处理。
* beforeDestroy  实例销毁之前调用。
* destroyed  Vue实例销毁之后调用:调用后，Vue实例的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

## 父子组件间的通信
### 父组件向子组件传值
```javascript
//父组件通过标签上面定义传值
<template>
    <Child :obj="msg"></Child>
</template>
<script>
    //引入子组件
    import Child form "./child"

    exprot default{
        name:"parent",
        data(){
            return {
                msg:"要向子组件传递的数据"
            }
        },
        //初始化组件
        components:{
            Child
        }
    }
</script>


//子组件通过props方法接受数据
<template>
    <h2>子组件</h2>
    <div>{{msg}}</div>
</template>
<script>
    exprot default{
        name:"child",
        //接受父组件传值
        props:["msg"]
    }
</script>
```
### 子组件向父组件传值
```javascript
//父组件向子组件传递事件方法，子组件通过$emit方法触发时间，回调给父组件  
<template>
   <child @msgfunc="func"></child>
</template>
<script>
    //引入子组件
    import Child form "./child"

    exprot default{
        methods:{
            func(msg){}
        },
        components:{
            Child
        }
    }
</script>

//子组件
<template>
    <button @click='handleClick'>点我</button>
</template>
<script>
    exprot default{
        name:"child",
        methods(){
            handleClick(){
                this.$emit('msgfunc')
            }
        }
    }
</script>
```
## Vue指令及用法
v-if：判断是否隐藏，控制的是这个 DOM 节点的存在与否  
v-show：仅仅控制元素的显示方式，将 display 属性在 block 和 none 来回切换  
>当我们需要经常切换某个元素的显示/隐藏时，使用v-show会更加节省性能上的开销；当只需要一次显示或隐藏时，使用v-if更加合理。

v-for：把数据遍历出来  
v-bind：用来绑定数据和属性以及表达式，缩写为‘：’  
v-model：实现双向绑定，使用在表单中，实现双向数据绑定的，在表单元素外使用不起作用  
  
## computed和watch和methods的使用和区别
代码中虽然可以在{{ }}中进行一些计算，但是当计算比较复杂时，写在模板中不是那么的友好，这时候就可以使用watch监听和computed计算、methods方法 属性将复杂的逻辑计算从模板中拆出来。
  
#### computed(计算属性) vs methods(方法)

同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。计算属性只有在它的相关依赖发生改变时才会重新执行函数求值。这就意味着只要 a 或 b 还没有发生改变，多次访问计算属性会立即返回之前的计算结果，而不必再次执行函数。  
这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖： computed: { now: function () { return Date.now() } }  
相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。  
  
#### computed(计算属性) vs watch(侦听属性)
Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：watch 属性。当你有一些数据需要随着其它数据的变动而变动时，很容易滥用 watch。 然而，通常更好的做法是使用计算属性而不是命令式的 watch 回调。  
watch实现的代码是命令式的和重复的使代码变得复杂。computed 计算属性实现的代码简洁易于页面的维护。  

## 双向数据绑定
#### vue数据双向绑定是通过数据劫持结合发布者-订阅者模式的方式来实现的。
其中数据劫持通过Object.defineProperty()来实现的。  
Object.defineProperty( )是用来做什么的？它可以来控制一个对象属性的一些特有操作，比如读写权、是否可以枚举，这里主要涉及到它对应的两个描述属性get和set。
```javascript
var Book = {}
var name = '';
Object.defineProperty(Book, 'name', {
  set: function (value) {
    name = value;
    console.log('你取了一个书名叫做' + value);
  },
  get: function () {
    return '《' + name + '》'
  }
})
// get就是在读取name属性这个值触发的函数，set就是在设置name属性这个值触发的函数

Book.name = 'vue权威指南';  // 你取了一个书名叫做vue权威指南
console.log(Book.name);  // 《vue权威指南》
```
实现mvvm主要包含两个方面，数据变化更新视图，视图变化更新数据。  
因为view更新data其实可以通过事件监听即可，比如input标签监听 'input' 事件就可以实现了。所以关键点是在于，当数据改变，如何更新视图的。  
  
数据更新视图的重点是如何知道数据变了，只要知道数据变了，那么接下去的事都好处理。  
如何知道数据变了，其实就是通过Object.defineProperty()对属性设置一个set函数，当数据改变了就会来触发这个函数，所以我们只要将一些需要更新的方法放在这里面就可以实现data更新view了。  
  
```javascript
// 简单实现方法
<body>
   <div id="app">
       <input type="text" id="txt">
       <p id="show-txt"></p>
   </div>
   <script>
       var obj = {}
       Object.defineProperty(obj, 'txt', {
           get: function () {
               return obj
           },
           set: function (newValue) {
               document.getElementById('txt').value = newValue
               document.getElementById('show-txt').innerHTML = newValue
           }
       })
       document.addEventListener('keyup', function (e) {
           obj.txt = e.target.value
       })
   </script>
</body>
```
总结实现数据的双向绑定3个步骤：
1.实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。  
2.实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。  
3.实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。  

## Vuex
Vuex——它是一个状态管理的工具。    
之前，我们是从顶层组件向下传递状态，而同胞组件之间并没有分享数据。如果它们需要相互通信，我们要在应用程序中推送状态。这是可以的，但是一旦你的程序变得复杂，这种方法就没有意义了。  
Vuex 是 Vue 中的 Redux。实际上，Redux 也可以用于 Vue，但是，使用专门为 Vue 设计的工具 Vuex 更加有利。  
#### state
* vuex 就是一个仓库，仓库里放了很多对象。其中 state 就是数据源存放地，对应于一般 vue 对象里面的 data  
* state 里面存放的数据是响应式的，vue 组件从 store 读取数据，若是 store 中的数据发生改变，依赖这相数据的组件也会发生更新
* 它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性
#### getter
* getter 可以对 state 进行计算操作，它就是 store 的计算属性
* 虽然在组件内也可以做计算属性，但是 getters 可以在多给件之间复用
* 如果一个状态只在一个组件内使用，是可以不用 getters
#### mutation
* mutation里面装着一些改变数据方法的集合，这是Veux设计很重要的一点，就是把处理数据逻辑方法全部放在mutations里面，使得数据和视图分离。
* Vuex中store数据改变的唯一方法就是mutation
#### action
* action 类似于 muation, 不同在于：action 提交的是 mutation,而不是直接变更状态
* action 可以包含任意异步操作

## keep-alive
<keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 <transition> 相似，<keep-alive> 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中。

生命钩子：activated 和 deactivated，主要用于保留组件状态或避免重新渲染。
因为keep-alive会将组件保存在内存中，并不会销毁以及重新创建，所以不会重新调用组件的created等方法，需要用activated与deactivated这两个生命钩子来得知当前组件是否处于活动状态。
<keep-alive> 是用在其一个直属的子组件被开关的情形。如果你在其中有 v-for 则不会工作。如果有上述的多个条件性的子元素，<keep-alive> 要求同时只有一个子元素被渲染。

参数：include 和 exclude 属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示，判断name为xxx的时候缓存/不缓存

max:
最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。


深入keep-alive组件实现
created钩子会创建一个cache对象，用来作为缓存容器，保存vnode节点。
destroyed钩子则在组件被销毁的时候清除cache缓存中的所有组件实例。
render()函数：
1 通过getFirstComponentChild获取第一个子组件，获取该组件的name（存在组件名则直接使用组件名，否则会使用tag）。接下来会将这个name通过include与exclude属性进行匹配，匹配不成功（说明不需要进行缓存）则不进行任何操作直接返回vnode，
2 
3 根据key在this.cache中查找，如果存在则说明之前已经缓存过了，直接将缓存的vnode的componentInstance（组件实例）覆盖到目前的vnode上面。否则将vnode存储在cache中。

## Vue-router
动态路由匹配
## VUE1 | VUE2 | VUE3 的区别
### vue1和vue2
### vue3
xxx
