## React学习笔记
### React编程特点
* 声明式开发： 面向数据编程，只需负责数据就可以，react自己会负责DOM的操作，减少DOM操作过程的代码量。
* 与其他框架共存： 例如react只负责入口文件里面id=root的那部分内容的DOM渲染，其他内容可由其他框架负责。
* 组件化：根据功能拆分成不同组件，分工明确。
* 单向数据流： 父组件被很多子组件引用的时候如果在一个子组件里被修改其他引用的也会跟着变，出错就难定位；父组件可以给子组件传值，子组件可以使用，但不能修改；要修改则是子组件去调用父组件传过来的方法去修改。
* 视图层框架： 组件很多时相互间通信麻烦，需引入readux等进行数据层框架解决通信问题。  
            so:只负责页面和数据之间的关系，不管怎么传值。  
* 函数式编程:  代码中写了很多函数，维护容易，分工明确,有利于自动化测试！因为函数的输入输出容易测试。


### propTypes和defaultProps
例子：const { content } = this.props;
propTypes: 进行属性接收的校验，父组件不能乱传东西  
步骤： 1.引入：import PropTypes from 'prop-types';  
      2.在调用了之后进行验证  
```javascript
TodoItem.protoType = {
  content: PropTypes.string，
  //isRequired表示index必须要传递，没传递值的话不会检测，不会报错
  index: PropTypes.number.isRequired 
  //要么是number要么是string
  deleteItem: PropTypes.oneOfType([PropTypes.number,PropTypes.string]),
}

defaultProps: = {
    index：123123 
    //给index默认值，如果isRequired检测到没传递的话就用默认值
}
```

### 代码执行流程：

1.数据和页面联动——当组件的state或者props发生改变时，render函数就会重新执行 (页面是render渲染出来的)。  
2.父组件的render函数被运行时，他的子组件的render都会被重新运行一次。  

### 虚拟DOM： 大大提高组件渲染的效率
 
方案1：缺陷——第一次生成完整的DOM片段，第二次也是，所以很耗能！
1.state: 数据  
2.JSX 模板  
3.数据 + 模板 结合，来生成真实的DOM 来显示  
4.state 发生改变  
5.数据 + 模板 结合，来生成真实的DOM 替换原始的DOM —> 耗能  
  
方案2：缺陷——性能提升不明显  
1.state: 数据  
2.JSX 模板  
3.数据 + 模板 结合，来生成真实的DOM 来显示  
4.state 发生改变  
5.数据 + 模板 结合，来生成真实的DOM 先不直接替换原始的DOM  
6.新的DOM（DocumentFragment），和老的DOM 对比,找差异 —> 也消耗了点性能  
7.找出input框发生的变化  
8.只用新的DOM中的input元素，替换老的DOM中的input元素  

#### 方案3：虚拟DOM（从比较DOM —> 比较JS对象）
1.state: 数据  
2.JSX 模板  
3.数据 + 模板 结合，来生成真实的DOM 来显示  
```javascript
    <div id="aaa"><span>hello world</span></div>  
```
4.生成虚拟DOM （虚拟DOM是一个JS对象，用它来描述真实的DOM）—> 也有消耗，但小：创建js对象简单，创建dom难  
```javascript
    ['div',{id:'aaa'}, ['span',{},'hello world']]  
    [ 'dom标签名称',{属性},[内容(child)-也是一个DOM]] 
```
5.state 发生变化  
6.数据 + 模板  生成新的虚拟DOM —> 极大提升性能  
```javascript
    ['div',{id:'aaa'}, ['span',{},'changed hahaha']]   
```
7.比较两个虚拟DOM的区别，找到区别是span内容 —> 对比操作的是js对象，提升性能  
8.直接操作DOM，修改span内容  


### 深入理解虚拟DOM

真实情况是先4步在3步:  
4.数据 + 模板 生成虚拟DOM （虚拟DOM是一个JS对象，用它来描述真实的DOM）—> 也有消耗，但是不大：创建js对象简单，创建dom难。   
```javascript
    ['div',{id:'aaa'}, ['span',{},'hello world']]  
    [ dom标签名称, 属性, 内容(child)-也是一个DOM ]  
```
3.用虚拟DOM的结构 来生成真实的DOM 来显示  
```javascript
    <div id="aaa"><span>hello world</span></div>  
```
render里面只是一个JSX模板 ，先生成JS对象 ，再生成真实DOM。   
```javascript
render() {
    //JSX -> createElement -> 虚拟DOM（JS对象）-> 真实的DOM
    //下面两行的效果是一样的
    return <div>item</div>
    return React.createElement('div', {}, 'item');

    return <div><span>item</span></div>
    return React.createElement('div', {}, React.createElement('span',{},'item'));
  }
```
优点：  
1.性能提升  
2.使跨端应用得以实现——React Native  
  移动端没有真实dom——原生应用中把虚拟DOM转化为原生的应用组件  
  网页才是转化为真实的DOM  


### 虚拟DOM的Diff（diffrence）算法简单介绍：
  
新的虚拟DOM和原先的虚拟DOM对比的一个算法。  
只有当数据发生改变（调用了setState的时候）才会进行对比。  
setState是异步的设计初衷——连续几个setState会合并成一个，只做一次虚拟DOM的对比和更新。  
  
概念：依次比对同层节点，一旦某层节点不同，则替换该节点下全部内容，这样虽然可能会造成重新渲染的一些浪费，但减少了时间，比对的速度快。  
  
key值会提高对比的速度——可以用item  
key值不要用index——节点发生变化的时候导致key值不稳定，会变化  

### React中的 ref
React中的 ref 帮助我们直接获取DOM元素（尽量少用）。  
```javascript
setState有两个参数 this.setState(()=>{},()=>{
    //回调函数，前面的执行完在执行这里，解决setState异步时间差问题
    //要获取页面更新后的DOM是，放在这里
})
```


### React生命周期函数：指在某一时刻组件会自动执行的函数
（每个组件都有自己的生命周期函数）  
```javascript
initialization(初始化)：setup props and state 

mounting(挂载)：  
    componentWillMount(){} ： 在组件即将被挂载到页面是自动执行，只执行一次  
    render(){} :更新页面  
    componentDidMount(){}  : 组件被挂载到页面的时候会自动执行，只执行一次 
    
updation(更新)：  
    states发生变化：
        shouldComponentUptate(){return true/false} ：组件被更新之前会自动执行
        componentWillUpdate(){} :组件被更新之前会执行，但他在shouldComponentUptate之后并且返回true时执行
        render(){} :更新页面
        componentDidUpdate(){} :组件更新之后被执行
        
    props发生变化：  
        componentWillReceiveProps(){} :当一个组件从父组件接收参数，并且
                                       只要父组件的render被重新执行时，这个周期函数就会执行
                                       （组件第一次存在父组件中不会执行）
        后面跟states情况一样.....
    
unmounting(去除)
        componentWillUnmount(){} :当组件即将从页面中被剔除是执行
```

#### 生命周期应用场景:

componentWillReceiveProps(){} ：可以避免无谓的render函数的运行，提升性能。  
  
componentDidMount(){} ：请求Ajax数据，不放在render里面是因为它会不断地重新运行，不放在componentWillMount里是当以后写react native或者用react写服务端重构的时候，放在这里会发生冲突。  
  
用axios请求数据：  
```javascript
    import axios from 'axios'
    componentDidMount(){
        axios.get('/api/todolist')
        .then(()=>{alert('success')})
        .catch(()=>{alert('error')})
    }
```

### 代码优化：
1.把作用域的修改放在constructor里面，保证整个模块这个函数的作用域只会绑定一次，而且可以避免子组件的一些无谓的渲染。  
2.React底层setState内置的性能提升机制：首先他是异步过程，会把几次请求合并成一次，降低虚拟DOM的比对频率。  
3.同层比对、key值设置来提升比对速度，从而提升性能。  
4.借助shouldComponentUptate周期函数避免无谓的render函数的运行。  
```javascript
    componentWillReceiveProps(nextProps,nextState) {
    if(nextProps.content !== this.props.content){
      return true;
    }else {
      return false;
    }
  }
```
### React实现css动画
插件：react transition group  
```javascript
import { CSSTransition,TransitionGroup } from 'react-transition-group'  
  
render() {
  return (
    <Fragment>
      <CSSTransition
        in={this.state.show}
        timeout={1000}
        classNames='fade'
        unmountOnExit
        onEntered={(el)=>{el.style.color='blue'}}
        appear={true}
      >
        <div>hello</div>
      </CSSTransition>
    </Fragment>
  )
}

.fade-enter {
  //入场动画第一个瞬间
}
.fade-enter-active {
  //入场动画第二个时刻——到动画执行完成
}
.fade-enter-done {
  //入场动画完成后
}

.fade-exit {
  //出场动画第一个时刻
}
.fade-exit-active {
  //出场动画第二个时刻——到动画执行完成
}
.fade-exit-done {
  //出场动画完成后
}

unmountOnExit  //动画完成后DOM被移除

onEntered={(el)=>{el.style.color='blue'}}
//可以通过钩子函数用js实现动画效果

onEnter/onEntering/onEntered
onExit/onExiting/onExited

appear={true} //页面出现是自动先执行一次动画
.fade-enter .fade-appear {
  //入场动画第一个瞬间
}
.fade-enter-active .fade-appear-active {
  //入场动画第二个时刻——到动画执行完成
}

单个动画用<CSSTransition></CSSTransition>
多个动画用<TransitionGroup><CSSTransition></CSSTransition></TransitionGroup>
```
