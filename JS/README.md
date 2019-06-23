* 内置类型
* H5语义化的理解
* new
* this
* 防抖(debounce)和节流(throttle)
* 闭包
* 深浅拷贝
* 模块化
* 继承
* 原型链
* 正则表达式
* 内置类型

## 内置类型
### JS中原始类型有哪几种？null 是对象吗？原始（基本）数据类型和复杂数据类型有什么区别？
目前，JS原始类型有六种，分别为:  
Boolean、String、Number、Undefined、Null、Symbol(ES6新增)  
ES10新增了一种基本数据类型：BigInt  
  
复杂数据类型只有一种: Object  
对象（Object）是引用类型，在使用过程中会遇到浅拷贝和深拷贝的问题。  
  
null 不是一个对象，尽管 typeof null 输出的是 object，这是一个历史遗留问题，JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象， null 表示为全零，所以将它错误的判断为 object 。另外，NaN 也属于 number 类型，并且 NaN 不等于自身。
#### 基本数据类型和复杂数据类型的区别为:
1、内存的分配不同  
基本数据类型存储在栈中。  
复杂数据类型存储在堆中，栈中存储的变量，是指向堆中的引用地址。  
2、访问机制不同  
基本数据类型是按值访问  
复杂数据类型按引用访问，JS不允许直接访问保存在堆内存中的对象，在访问一个对象时，首先得到的是这个对象在堆内存中的地址，然后再按照这个地址去获得这个对象中的值。  
3复制变量时不同(a=b)  
基本数据类型：a=b;是将b中保存的原始值的副本赋值给新变量a，a和b完全独立，互不影响。  
复杂数据类型：a=b;将b保存的对象内存的引用地址赋值给了新变量a;a和b指向了同一个堆内存地址，其中一个值发生了改变，另一个也会改变。  
```javascript
let b = {
  age: 10
}
let a = b;
a.age = 20;
console.log(b); 
//{ age: 20 }
```
#### 四则运算:
```javascript
1 + '1'   // '11'
2 * '2'   // 4
[1, 2] + [2, 1]   // '1,22,1'
[1, 2].toString() // '1,2'
[2, 1].toString() // '2,1'
'1,2' + '2,1'     // '1,22,1'

'a' + + 'b' // -> "aNaN"
// 因为 + 'b' -> NaN
// 你也许在一些代码中看到过 + '1' -> 1
```
#### == 和 === 有的区别
=== 不需要进行类型转换，只有类型相同并且值相等时，才返回 true.  
== 如果两者类型不同，首先需要进行类型转换。具体流程如下:  
  
双等号==:  
* （1）如果两个值类型相同，再进行三个等号(===)的比较  
* （2）如果两个值类型不同，也有可能相等，需根据以下规则进行类型转换在比较：  
*  a、如果一个是null，一个是undefined，那么相等  
*  b、判断两者类型是否为 string 和 number, 如果是, 将字符串转换成 number  
*  c、判断其中一方是否为 boolean, 如果是, 将 boolean 转为 number 再进行判断  
*  d、判断其中一方是否为 object 且另一方为 string、number 或者 symbol , 如果是, 将 object 转为原始类型再进行判断  
  
三等号===:  
* （1）如果类型不同，就一定不相等  
* （2）如果两个都是数值，并且是同一个值，那么相等；如果其中至少一个是NaN，那么不相等。（判断一个值是否是NaN，只能使用isNaN( ) 来判断）  
* （3）如果两个都是字符串，每个位置的字符都一样，那么相等，否则不相等  
* （4）如果两个值都是true，或是false，那么相等  
* （5）如果两个值都引用同一个对象或是函数，那么相等，否则不相等  
* （6）如果两个值都是null，或是undefined，那么相等  

#### typeof 是否正确判断类型? instanceof呢？ instanceof 的实现原理是什么？
首先 typeof 能够正确的判断基本数据类型，但是除了 null, typeof null输出的是对象。  
但是对象来说，typeof 不能正确的判断其类型， typeof 一个函数可以输出 'function',而除此之外，输出的全是 object,这种情况下，我们无法准确的知道对象的类型。  
  
instanceof可以准确的判断复杂数据类型，但是不能正确判断基本数据类型。  
instanceof 是通过原型链判断的，A instanceof B, 在A的原型链中层层查找，是否有原型(prototype)等于 B.__proto__，如果一直找到A的原型链的顶端(null;即 Object.prototype.__proto__),仍然不等于B.prototype，那么返回false，否则返回true。 
#### 自己手动实现一下 instanceof
```javascript
function instanceof(left, right) {
  // 获得类型的原型
  let prototype = right.prototype
  // 获得对象的原型
  left = left.__proto__
  // 判断对象的类型是否等于类型的原型
  while (true) {
   if (left === null)
   	return false
   if (prototype === left)
   	return true
    left = left.__proto__
  }
}
```
#### 如何判断一个变量是不是数组？
1.使用 Array.isArray 判断，如果返回 true, 说明是数组。  
2.使用 instanceof Array 判断，如果返回true, 说明是数组。  
3.使用 Object.prototype.toString.call 判断，如果值是 [object Array], 说明是数组。  
4.通过 constructor 来判断，如果是数组，那么 arr.constructor===Array。 (不准确，因为我们可以指定 obj.constructor=Array)  

## H5语义化的理解
语义化意味着顾名思义，HTML5的语义化指的是合理正确的使用语义化的标签来创建页面结构，如 header,footer,nav，从标签上即可以直观的知道这个标签的作用，而不是滥用div。  
#### 语义化的优点有:
代码结构清晰，易于阅读，利于开发和维护。  
方便其他设备解析（如屏幕阅读器）根据语义渲染网页。  
有利于搜索引擎优化（SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重  
  
#### 语义化标签主要有：
title：主要用于页面的头部的信息介绍  
header：定义文档的页眉  
nav：主要用于页面导航  
main：规定文档的主要内容。对于文档来说应当是唯一的。它不应包含在文档中重复出现的内容，比如侧栏、导航栏、版权信息、站点标志或搜索表单。  
article：独立的自包含内容  
h1~h6：定义标题  
ul: 用来定义无序列表  
ol: 用来定义有序列表  
address：定义文档或文章的作者/拥有者的联系信息。  
canvas：用于绘制图像  
dialog：定义一个对话框、确认框或窗口  
aside：定义其所处内容之外的内容。例如可用作文章的侧栏。  
section：定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。  
figure：规定独立的流内容（图像、图表、照片、代码等等）。figure 元素的内容应该与主内容相关，但如果被删除，则不应对文档流产生影响。  
details：描述文档或者文档某一部分细节  
mark：义带有记号的文本。  
## new一个对象的具体过程
1. 创建空对象；  
　　var obj = {};  
2. 设置新对象的constructor属性为构造函数的名称，设置新对象的__proto__属性指向构造函数的prototype对象；  
　　obj.__proto__ = ClassA.prototype;  
3. 使用新对象调用函数，函数中的this被指向新实例对象：  
　　ClassA.call(obj);　　//{}.构造函数();  
4. 将初始化完毕的新对象地址，保存到等号左边的变量中。  
#### 自己手动实现一个new:
```javascript
function create() {
    // 创建一个空的对象
    let obj = new Object()
    // 获得构造函数
    let Con = [].shift.call(arguments)
    // 链接到原型
    obj.__proto__ = Con.prototype
    // 绑定 this，执行构造函数
    let result = Con.apply(obj, arguments)
    // 确保 new 出来的是个对象
    return typeof result === 'object' ? result : obj
}
```
## this: 如何正确判断this的指向?
1. 全局环境中的 this  
浏览器环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象 window;  
node 环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部），this 都是空对象 {}。  
非严格模式： node环境，执行全局对象 global，浏览器环境，执行全局对象 window。  
严格模式：执行 undefined。  
```javascript
function info(){
  console.log(this.age);
}
var age = 28;
info(); 
//严格模式抛错, 因为 this 此时是 undefined
//非严格模式，node下输出 undefined（因为全局的age不会挂在 global 上）
//非严格模式。浏览器环境下输出 28（因为全局的age挂在 window 上）
```
2. 是否是 new 绑定  
如果是 new 绑定，并且构造函数中没有返回 function 或者是 object，那么 this 指向这个新对象。  
如果构造函数返回值是 function 或 object，这种情况下 this 指向的是返回的对象。  
```javascript
function Super(age) {
  this.age = age;
}
let instance = new Super('6');
console.log(instance.age);  //6

function Super(age) {
  this.age = age;
  let obj = {a: '2'};
  return obj;
}
let instance = new Super('hello');
console.log(instance.age);   //undefined
```
3. 函数是否通过 call,apply 调用，或者使用了 bind 绑定，如果是，那么this绑定的就是指定的对象。  
4. 隐式绑定，函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的隐式调用为: xxx.fn()
```javascript
function info(){
  console.log(this.age);
}
var person = {
  age: 20,
  info
}
var age = 28;
person.info(); //20;执行的是隐式绑定
```
5. 箭头函数的情况：箭头函数没有自己的this，继承外层上下文绑定的this。  
```javascript
let obj = {
  age: 10,
  info: function() {
    return () => {
      console.log(this.age);  //this继承的是外层上下文绑定的this
    }
  }
}
let person = {age: 18};
let info = obj.info();
info();    //10

let info2 = obj.info.call(person);
info2();   //18
```
## 防抖(debounce)和节流(throttle)
防抖和节流的作用都是防止函数多次调用。  
区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于设置的时间，防抖的情况下只会调用一次，而节流的情况会每隔一定时间调用一次函数。  

### 防抖函数的作用是什么？有哪些应用场景，请实现一个防抖函数。
防抖函数的作用就是控制函数在一定时间内的执行次数。防抖意味着N秒内函数只会被执行一次，如果N秒内再次被触发，则重新计算延迟时间。  
  
#### 防抖应用场景：  
1、搜索框输入查询，如果用户一直在输入中，没有必要不停地调用去请求服务端接口，等用户停止输入的时候，再调用，设置一个合适的时间间隔，有效减轻服务端压力。  
2、表单验证。  
3、按钮提交事件。  
4、浏览器窗口缩放，resize事件等。  
#### 防抖函数实现：
```javascript
// immediate 为 true 时，表示开始会立即触发一次。
// immediate 为 false 时，表示最后一次一定会触发。
// loadsh 中的 debounce 的第三个参数 option ，提供了 leading 和 trailing两个参数。

function debounce(func, wait, immediate=true) {
  let timeout, context, args;
  // 延迟执行函数
  const later = () => setTimeout(() => {
    // 延迟函数执行完毕，清空定时器
    timeout = null
    // 延迟执行的情况下，函数会在延迟函数中执行
    // 使用到之前缓存的参数和上下文
    if (!immediate) {
        func.apply(context, args);
        context = args = null;
    }
  }, wait);
  let debounced = function (...params) {
    if (!timeout) {
    timeout = later();
    if (immediate) {
      //立即执行
      func.apply(this, params);
    } else {
      //闭包
      context = this;
      args = params;
      }
    } else {
      clearTimeout(timeout);
      timeout = later();
    }
  }
  debounced.cancel = function () {
    clearTimeout(timeout);
    timeout = null;
  };
  return debounced;
};

```
### 节流函数的作用是什么？有哪些应用场景，请实现一个防抖函数。
节流函数的作用是规定一个单位时间，在这个单位时间内最多只能触发一次函数执行，如果这个单位时间内多次触发函数，只能有一次生效。  
#### 函数节流的应用场景有:
1.DOM 元素的拖拽功能实现（mousemove）  
2.射击游戏的 mousedown/keydown 事件（单位时间只能发射一颗子弹）  
3.计算鼠标移动的距离（mousemove）  
4.监听滚动事件判断是否到页面底部自动加载更多：给 scroll 加了 debounce 后，只有用户停止滚动后，才会判断是否到了页面底部；如果是 throttle 的话，只要页面滚动就会间隔一段时间判断一次  
```javascript
function throttle(func, wait, options) {
    var timeout, context, args, result;
    var previous = 0;
    if (!options) options = {};

    var later = function () {
        previous = options.leading === false ? 0 : Date.now() || new Date().getTime();
        timeout = null;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
    };

    var throttled = function () {
        var now = Date.now() || new Date().getTime();
        if (!previous && options.leading === false) previous = now;
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            result = func.apply(context, args);
            if (!timeout) context = args = null;
        } else if (!timeout && options.trailing !== false) {
            // 判断是否设置了定时器和 trailing
            timeout = setTimeout(later, remaining);
        }
        return result;
    };

    throttled.cancel = function () {
        clearTimeout(timeout);
        previous = 0;
        timeout = context = args = null;
    };

    return throttled;
}
```
## 闭包
闭包是指有权访问另一个函数作用域中的变量的函数，创建闭包最常用的方式就是在一个函数内部创建另一个函数。
```javascript
function foo() {
  var a = 2;
  return function fn() {
    console.log(a);
  }
}
let func = foo();
func(); //输出2
```
#### 闭包的缺点：
闭包会导致函数的变量一直保存在内存中，过多的闭包可能会导致内存泄漏。
```javascript
// 经典面试题，循环中使用闭包解决 var 定义函数的问题
for ( var i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
// 首先因为 setTimeout 是个异步函数，所有会先把循环全部执行完毕，这时候 i 就是 6 了，所以会输出一堆 6。

// 解决办法:
第一种使用立即执行函数
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    }, j * 1000);
  })(i);
}

第二种就是使用 setTimeout 的第三个参数
for ( var i=1; i<=5; i++) {
	setTimeout( function timer(j) {
		console.log( j );
	}, i*1000, i);
}

第三种就是使用 let 定义 i 了
for ( let i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```
## 深浅拷贝
### 浅拷贝
简单的赋值就是浅拷贝，因为对象和数组在赋值的时候都是引用传递，赋值的时候只是传递一个指针。
```javascript
// 首先可以通过 Object.assign 来解决这个问题。
let a = {
    age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1

// 当然我们也可以通过展开运算符（…）来解决
let a = {
    age: 1
}
let b = {...a}
a.age = 2
console.log(b.age) // 1
```
### 深拷贝
浅拷贝很容易，但是很多时候我们需要原样的把数组或者对象复制一份，在修改值的时候，不改变初始对象的值；  
或者拷贝的对象只解决了第一层的问题，如果接下去的值中还有对象的话 这个时候就需要使用深拷贝。  
  
对于数组我们可以使用slice()和concat()方法来解决上面的问题  
```javascript
silce：  
var arr = ['wsscat', 'autumns', 'winds'];
        var arrCopy = arr.slice(0);
        arrCopy[0] = 'tacssw'
        console.log(arr)      //['wsscat', 'autumns', 'winds']
        console.log(arrCopy)  //['tacssw', 'autumns', 'winds']
  
concat：  
var arr = ['wsscat', 'autumns', 'winds'];
        var arrCopy = arr.concat();
        arrCopy[0] = 'tacssw'
        console.log(arr)      //['wsscat', 'autumns', 'winds']
        console.log(arrCopy)  //['tacssw', 'autumns', 'winds']
```
对象  
对象我们可以定义一个新的对象并遍历新的属性上去实现深拷贝  
```
var obj = {
            name: 'wsscat',
            age: 0
        }
        var deepCopy = function(source) {
            var result = {};
            for(var key in source) {
                if(typeof source[key] === 'object') {
                    result[key] = deepCopy(source[key])
                } else {
                    result[key] = source[key]
                }
            }
            return result;
        }
        var obj3 = deepCopy(obj)
        obj.name = 'autumns';
        console.log(obj);//Object {name: "autumns", age: 0}
        console.log(obj3);//Object {name: "wsscat", age: 0}
```

## 模块化
## 继承
## 原型链
## 正则表达式
