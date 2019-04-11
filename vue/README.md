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
