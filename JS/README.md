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
>>（1）如果两个值类型相同，再进行三个等号(===)的比较  
>>（2）如果两个值类型不同，也有可能相等，需根据以下规则进行类型转换在比较：  
>>>> 1）如果一个是null，一个是undefined，那么相等  
>>>> 2）如果一个是字符串，一个是数值，把字符串转换成数值之后再进行比较  
三等号===:  
>>（1）如果类型不同，就一定不相等  
>>（2）如果两个都是数值，并且是同一个值，那么相等；如果其中至少一个是NaN，那么不相等。（判断一个值是否是NaN，只能使用isNaN( ) 来判断）  
>>（3）如果两个都是字符串，每个位置的字符都一样，那么相等，否则不相等  
>>（4）如果两个值都是true，或是false，那么相等  
>>（5）如果两个值都引用同一个对象或是函数，那么相等，否则不相等  
>>（6）如果两个值都是null，或是undefined，那么相等  

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
