* 内置类型
* H5语义化的理解
* new
* this
* 防抖(debounce)和节流(throttle)
* 闭包
* 深浅拷贝
* 模块化
* 原型和原型链
* 继承
* 正则表达式


## 内置类型
### JS中原始类型有哪几种？null 是对象吗？原始（基本）数据类型和复杂数据类型有什么区别？
目前，JS原始类型有六种，分别为:  
Boolean、String、Number、Undefined、Null、Symbol(ES6新增)  
ES10新增了一种基本数据类型：BigInt  
  
复杂数据类型只有一种: Object(数据是对象，函数是对象，正则表达式也是对象)  
对象（Object）是引用类型，在使用过程中会遇到浅拷贝和深拷贝的问题。  
  
null 不是一个对象，尽管 typeof null 输出的是 object，这是一个历史遗留问题，JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象， null 表示为全零，所以将它错误的判断为 object 。另外，NaN 也属于 number 类型，并且 NaN 不等于自身。  

#### symbol：
ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。  
Symbol 值通过Symbol函数生成。  
```javascript
let s = Symbol();
typeof s    //“symbol”，Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。

let s1 = Symbol('foo');
s1 // Symbol(foo)
s1.toString() // "Symbol(foo)"   //加上参数，易于区分

// 参数是对象
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)

Symbol 值可以显式转为字符串，布尔值，不能转为数值/参与运算

String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'
!sym  // false
"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string

// 方法：
sym.description // "foo"
Symbol.hasInstance
方法，会被instanceof运算符调用。构造器对象用来识别一个对象是否是其实例
Symbol.match
方法，被String.prototype.match调用。正则表达式用来匹配字符串。
Symbol.replace
方法，被String.prototype.replace调用。正则表达式用来替换字符串中匹配的子串。
Symbol.search
方法，被String.prototype.search调用。正则表达式返回被匹配部分在字符串中的索引。
Symbol.species
函数值，为一个构造函数。用来创建派生对象。
Symbol.split
方法，被String.prototype.split调用。正则表达式来用分割字符串。
```

#### 基本数据类型和复杂数据类型的区别为:
1、内存的分配不同  
基本数据类型存储在栈中。  
复杂数据类型的内存空间为栈,内存空间中存储了指针,该指针指向了堆中存储的Object类型数据，解释器启动时会先寻找栈中的地址，取得地址后从堆中取得对应的实体。  
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

// 引用类型的比较是引用的比较(堆的空间)
var a = [1,2,3];
var b = [1,2,3];
console.log(a === b); // false
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
首先 typeof 能够正确的判断基本数据类型，但是除了 null: typeof null输出的是对象。  
但是对象来说，typeof 不能正确的判断其类型， typeof 一个函数可以输出 'function',而除此之外，输出的全是 object,这种情况下，我们无法准确的知道对象的类型。  
  
instanceof可以准确的判断复杂数据类型，但是不能正确判断基本数据类型。  
instanceof 是通过原型链判断的，A instanceof B, 在A的原型链中层层查找，是否有原型(prototype)等于 B.prototype，如果一直找到A的原型链的顶端(null;即 Object.prototype.__proto__),仍然不等于B.prototype，那么返回false，否则返回true。 
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
  
常可以通过 JSON.parse(JSON.stringify(object)) 来解决。  
```javascript
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE

但是该方法也是有局限性的：
1.会忽略 undefined
2.会忽略 symbol
3.不能复制函数、正则对象
4.不能解决循环引用的对象
```
数组  
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
        console.log(obj);   //Object {name: "autumns", age: 0}
        console.log(obj3);  //Object {name: "wsscat", age: 0}
```

## 模块化
### CommonJS
CommonJS是第一个流行的模块化规范却由服务器端的JavaScript应用带来，CommonJS规范是由NodeJS发扬光大，这标志着JavaScript模块化编程正式登上舞台。  
一，定义模块:  
根据CommonJS规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为global对象的属性。  
二，模块输出：  
模块只有一个出口，module.exports对象，我们需要把模块希望输出的内容放入该对象。  
三，加载模块：  
加载模块使用require方法，该方法读取一个文件并执行，返回文件内部的module.exports对象。  
```
var name = 'Byron';
function printName(){
    console.log(name);
}
 
function printFullName(firstName){
    console.log(firstName + name);
}
 
module.exports = {
    printName: printName,
    printFullName: printFullName
}

///////////////////////
// 加载模块
var nameModule = require('./myModel.js');
nameModule.printName();
```
### AMD
AMD 即Asynchronous Module Definition，异步模块定义的意思。它是一个在浏览器端模块化开发的规范，由于不是JavaScript原生支持，使用AMD规范进行页面开发需要用到对应的库函数，也就是RequireJS，实际上AMD 是 RequireJS 在推广过程中对模块定义的规范化的产出。  
requireJS主要解决两个问题：  
一，多个js文件可能有依赖关系，被依赖的文件需要早于依赖它的文件加载到浏览器；  
二，js加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间越长。  
```
// 定义模块 myModule.js
define(['dependency'], function(){
    var name = 'Byron';
    function printName(){
        console.log(name);
    }
 
    return {
        printName: printName
    };
});
 
// 加载模块
require(['myModule'], function (my){
　 my.printName();
});
```
### CMD
CMD 即Common Module Definition通用模块定义，CMD规范是国内发展出来的，就像AMD有个requireJS，CMD有个浏览器的实现SeaJS，SeaJS要解决的问题和requireJS一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。  
语法:  
Sea.js 推崇一个模块一个文件，遵循统一的写法  
define  
define(id?, deps?, factory)  
因为CMD推崇一个文件一个模块，所以经常就用文件名作为模块id；CMD推崇依赖就近，所以一般不在define的参数中写依赖，而是在factory中写。  
factory有三个参数：  
function(require, exports, module){}  
一，require  
require 是 factory 函数的第一个参数，require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口；  
二，exports  
exports 是一个对象，用来向外提供模块接口；  
三，module  
module 是一个对象，上面存储了与当前模块相关联的一些属性和方法。  
```
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js')
  $('div').addClass('active');
});

// 加载模块
seajs.use(['myModule.js'], function(my){

});
```
### AMD与CMD区别
1、最明显的区别就是在模块定义时对依赖的处理不同：  
AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块，  
CMD推崇就近依赖，只有在用到某个模块的时候再去require，  
这种区别各有优劣，只是语法上的差距，而且requireJS和SeaJS都支持对方的写法。  
  
2、AMD和CMD最大的区别是对依赖模块的执行时机处理不同，注意不是加载的时机或者方式不同。  
很多人说requireJS是异步加载模块，SeaJS是同步加载模块，这么理解实际上是不准确的，其实加载模块都是异步的，只不过AMD依赖前置，js可以方便知道依赖模块是谁，立即加载，而CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。  
为什么我们说两个的区别是依赖模块执行时机不同，为什么很多人认为ADM是异步的，CMD是同步的（除了名字的原因）  
同样都是异步加载模块，AMD在加载模块完成后就会执行改模块，所有模块都加载执行完后会进入require的回调函数，执行主逻辑，这样的效果就是依赖模块的执行顺序和书写顺序不一定一致，看网络速度，哪个先下载下来，哪个先执行，但是主逻辑一定在所有依赖加载完成后才执行。  
CMD加载完某个依赖模块后并不执行，只是下载而已，在所有依赖模块加载完成后进入主逻辑，遇到require语句的时候才执行对应的模块，这样模块的执行顺序和书写顺序是完全一致的。  
这也是很多人说AMD用户体验好，因为没有延迟，依赖模块提前执行了，CMD性能好，因为只有用户需要的时候才执行的原因。  

## 原型和原型链
#### 一: 函数对象
* 所有引用类型（函数，数组，对象）都拥有__proto__属性（隐式原型）
* 所有函数拥有prototype属性（显式原型）（仅限函数）
* 原型对象：拥有prototype属性的对象，在定义函数时就被创建
#### 二: 构造函数
```javascript
       //创建构造函数
        function Word(words){
            this.words = words;
        }
        Word.prototype = {
            alert(){
                alert(this.words);
            }
        }
        //创建实例
        var w = new Word("hello world");
        w.print = function(){
            console.log(this.words);
            console.log(this);  //Person对象
        }
        w.print();  //hello world
        w.alert();  //hello world
```
print()方法是w实例本身具有的方法，所以w.print()打印hello world；alert()不属于w实例的方法，属于构造函数的方法，w.alert()也会打印hello world，因为实例继承构造函数的方法。  
  
实例w的隐式原型指向它构造函数的显式原型，指向的意思是恒等于 w.__proto__ === Word.prototype  
  
 当调用某种方法或查找某种属性时，首先会在自身调用和查找，如果自身并没有该属性或方法，则会去它的__proto__属性中调用查找，也就是它构造函数的prototype中调用查找。所以很好理解实例继承构造函数的方法和属性：  
w本身没有alert()方法，所以会去Word()的显式原型中调用alert()，即实例继承构造函数的方法。  
  
#### 三: 原型和原型链
```javascript
Function.prototype.a = "a";
Object.prototype.b = "b";
function Person(){}
console.log(Person);    //function Person()
let p = new Person();
console.log(p);         //Person {} 对象
console.log(p.a);       //undefined
console.log(p.b);       //b
```
注意p.a打印结果为undefined，p.b结果为b  
解析：  
p是Person()的实例，是一个Person对象，它拥有一个属性值__proto__，并且__proto__是一个对象，包含两个属性值constructor和__proto__  
```javascript
        console.log(p.__proto__.constructor);   //function Person(){}
        console.log(p.__proto__.__proto__);     //对象{}，拥有很多属性值
```
会发现p.__proto__.constructor返回的结果为构造函数本身，p.__proto__.__proto__有很多参数  
调用constructor属性，p.__proto__.__proto__.constructor得到拥有多个参数的Object()函数，Person.prototype的隐式原型的constructor指向Object()，即Person.prototype.__proto__.constructor == Object()  
从p.__proto__.constructor返回的结果为构造函数本身得到Person.prototype.constructor == Person()  
所以p.__proto__.__proto__== Object.prototype  
所以p.b打印结果为b，p没有b属性，会一直通过__proto__向上查找，最后当查找到Object.prototype时找到，最后打印出b，向上查找过程中，得到的是Object.prototype，而不是Function.prototype，找不到a属性，所以结果为undefined，这就是原型链，通过__proto__向上进行查找，最终到null结束  
```javascript
 console.log(p.__proto__.__proto__.__proto__);   //null
 console.log(Object.prototype.__proto__);        //null
```
```javascript
function Function(){}
console.log(Function);  //Function()
console.log(Function.prototype.constructor);    //Function()
console.log(Function.prototype.__proto__);      //Object.prototype
console.log(Function.prototype.__proto__.__proto__);    //NULL
console.log(Function.prototype.__proto__.constructor);  //Object()
console.log(Function.prototype.__proto__ === Object.prototype); //true
```
#### 总结：
1.查找属性，如果本身没有，则会去__proto__中查找，也就是构造函数的显式原型中查找，如果构造函数中也没有该属性，因为构造函数也是对象，也有__proto__，那么会去它的显式原型中查找，一直到null，如果没有则返回undefined  
2.p.__proto__.constructor  == function Person(){}  
3.p.___proto__.__proto__ == Object.prototype  
4.p.___proto__.__proto__.__proto__ == Object.prototype.__proto__ == null  
5.通过__proto__形成原型链而非protrotype  

## 继承
JS作为面向对象的弱类型语言，继承也是其非常强大的特性之一。
```javascript
// 统一 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
```
#### 1、原型链继承
核心： 将父类的实例作为子类的原型
```javascript
function Cat(){ 
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
```
特点：  
非常纯粹的继承关系，实例是子类的实例，也是父类的实例  
父类新增原型方法/原型属性，子类都能访问到  
简单，易于实现  
缺点：  
要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行，不能放到构造器中  
无法实现多继承  
来自原型对象的所有属性被所有实例共享（来自原型对象的引用属性是所有实例共享的）  
创建子类实例时，无法向父类构造函数传参  

#### 2、构造继承
核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}

var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```
特点：  
解决了1中，子类实例共享父类引用属性的问题  
创建子类实例时，可以向父类传递参数  
可以实现多继承（call多个父类对象）  
缺点：  
实例并不是父类的实例，只是子类的实例  
只能继承父类的实例属性和方法，不能继承原型属性/方法  
无法实现函数复用，每个子类都有父类实例函数的副本，影响性能  

#### 3、实例继承
核心：为父类实例添加新特性，作为子类实例返回
```javascript
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}

var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // false
```
特点：  
不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果  
缺点：  
实例是父类的实例，不是子类的实例  
不支持多继承  

#### 4、拷贝继承
```javascript
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}

var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```
特点：  
支持多继承  
缺点：  
效率较低，内存占用高（因为要拷贝父类的属性）  
无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）  

#### 5、组合继承
核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
```
特点：  
弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法  
既是子类的实例，也是父类的实例  
不存在引用属性共享问题  
可传参  
函数可复用  
缺点：  
调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）  

#### 6、寄生组合继承
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();

var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
Cat.prototype.constructor = Cat; // 需要修复下构造函数
```
特点：  
堪称完美  
缺点：  
实现较为复杂  

## 正则表达式
#### 代表特殊含义的元字符
\d : 0-9之间的任意一个数字  \d只占一个位置  
\w : 数字，字母 ，下划线 0-9 a-z A-Z _  
\s : 空格或者空白等  
\D : 除了\d  
\W : 除了\w  
\S : 除了\s  
 . : 除了\n之外的任意一个字符  
 \ : 转义字符  
 | : 或者  
() : 分组  
\n : 匹配换行符  
\b : 匹配边界 字符串的开头和结尾 空格的两边都是边界 => 不占用字符串位数  
 ^ : 限定开始位置 => 本身不占位置  
 $ : 限定结束位置 => 本身不占位置  
[a-z] : 任意字母 []中的表示任意一个都可以  
[^a-z] : 非字母 []中^代表除了  
[abc] : abc三个字母中的任何一个 [^abc]除了这三个字母中的任何一个字符  
  
#### 代表次数的量词元字符
\* : 0到多个  
\+ : 1到多个  
? : 0次或1次 可有可无  
{n} : 正好n次；  
{n,} : n到多次  
{n,m} : n次到m次  

#### 方法：test、exec、match和replace
reg.test(str) 用来验证字符串是否符合正则 符合返回true 否则返回false
```javascript
var str = 'abc';
var reg = /\w+/;
console.log(reg.test(str));  //true
```
reg.exec() 用来捕获符合规则的字符串
```javascript
var str = 'abc123cba456aaa789';
var reg = /\d+/;
console.log(reg.exec(str))
//  ["123", index: 3, input: "abc123cba456aaa789"];
console.log(reg.lastIndex)
// lastIndex : 0 

reg.exec捕获的数组中 
没有加'g'标识符，则exec捕获的每次都是同一个

// [0:"123",index:3,input:"abc123cba456aaa789"]
0:"123" 表示我们捕获到的字符串
index:3 表示捕获开始位置的索引
input 表示原有的字符串
lastIndex ：这个属性记录的就是下一次捕获从哪个索引开始
```
str.match(reg) 如果匹配成功，就返回匹配成功的数组，如果匹配不成功，就返回null
```javascript
var str = 'abc123cba456aaa789';
var reg = /\d+/g;
console.log(reg.exec(str));
// ["123", index: 3, input: "abc123cba456aaa789"]
console.log(str.match(reg));
// ["123", "456", "789"]

当全局匹配时，match方法会一次性把符合匹配条件的字符串全部捕获到数组中,
如果想用exec来达到同样的效果需要执行多次exec方法。
```
str.replace() 匹配字符串，匹配成功的字符去替换成新的字符串
```javascript
var str = '2017-01-06';
str = str.replace(/-(\d+)/g,function(){
    console.log(arguments)
})
["-01", "01", 4, "2017-01-06"]
["-06", "06", 7, "2017-01-06"]
"2017undefinedundefined"
```
通过replace方法获取url中的参数的方法
```javascript
(function(pro){
    function queryString(){
        var obj = {},
            reg = /([^?&#+]+)=([^?&#+]+)/g;
        this.replace(reg,function($0,$1,$2){
            obj[$1] = $2;
        })
        return obj;
    }
    pro.queryString = queryString;
}(String.prototype));

// 例如 url为 https://www.baidu.com?a=1&b=2
// window.location.href.queryString();
// {a:1,b:2}
```
#### 常用正则表达式：
```javascript
验证用户密码：(正确格式为：以字母开头，长度在6~18之间，只能包含字符、数字和下划线)
"^[a-zA-Z]\w{5,17}$" 

验证是否含有^%&',;=?$\"等字符：
"[^%&',;=?$\x22]+"

只能输入汉字：
"^[\u4e00-\u9fa5]{0,}$"

验证Email地址：
"^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$"

验证URL：
"^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$"

验证电话号码：(正确格式为："XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX")
"^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$"

验证身份证号（15位或18位数字）：
"^\d{15}|\d{18}$"

手机号正则
/(^1[3|4|5|7|8]\d{9}$)|(^09\d{8}$)/
```
