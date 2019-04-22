## ES6-ES10新特性
ES6中的特性比较多，在这里列举常用的：
* 类
* 模块化
* 箭头函数
* 模板字符串
* 解构赋值和延展操作符
* 对象属性简写
* Let与Const
* Promise

ES7新特性：
* 数组include()方法
* a ** b指数操作运算符

ES8新特性：
* async/await
* 

ES9新特性：

ES10新特性：

### 类（class）

对熟悉Java、c++等纯面向对象语言来说，都会有class这个概念,作为对象的模板,通过class关键字,可以定义类。  
ES6 引入了class（类），让JavaScript的面向对象编程变得更加简单和易于理解。  
```javascript
class Person{              //定义了一个名字为Person的类
    constructor(name,age){ //constructor是一个构造方法，用来接收参数
        this.name = name;  //this代表的是实例对象
        this.age=age;
    }
    say(){                  //这是一个类的方法，注意千万不要加上function
        return "我的名字叫" + this.name+"今年"+this.age+"岁了";
    }
}
var obj=new Person("laotie",88);
console.log(obj.say());     //我的名字叫laotie今年88岁了
```

由下面代码可以看出类实质上就是一个函数。  
类自身指向的就是构造函数。所以可以认为ES6中的类其实就是构造函数的另外一种写法！  
```javascript
console.log(typeof Person);                           //function
console.log(Person===Person.prototype.constructor);   //true
```
以下代码说明构造函数的prototype属性，在ES6的类中依然存在着。  
实际上类的所有方法都定义在类的prototype属性上。  
```javascript
console.log(Person.prototype);//输出的结果是一个对象
```
当然也可以通过prototype属性对类添加方法
```javascript
Person.prototype.addFn=function(){
    return "我是通过prototype新增加的方法,名字叫addFn";
}
var obj=new Person("laotie",88);
console.log(obj.addFn());   //我是通过prototype新增加的方法,名字叫addFn
```
还可以通过Object.assign方法来为对象动态增加方法
```javascript
Object.assign(Person.prototype,{
    getName:function(){
        return this.name;
    },
    getAge:function(){
        return this.age;
    }
})
var obj=new Person("laotie",88);
console.log(obj.getName());  //laotie
console.log(obj.getAge());   //88
```
* constructor方法是类的构造函数的默认方法，即使你没有添加构造函数，构造函数也是存在的，通过new命令生成对象实例时，自动调用该方法。  
* constructor方法默认返回实例对象this，但是也可以指定constructor方法返回一个全新的对象，让返回的实例对象不是该类的实例。
* constructor中定义的属性可以称为实例属性（即定义在this对象上）。
* constructor外声明的属性都是定义在原型上的，可以称为原型属性（即定义在class上)。
* hasOwnProperty()函数用于判断属性是否是实例属性。其结果是一个布尔值， true说明是实例属性，false说明不是实例属性。
* in操作符会在通过对象能够访问给定属性时返回true,无论该属性存在于实例中还是原型中。
```javascript
class Box{
    constructor(num1,num2){
        this.num1 = num1;
        this.num2=num2;
    }
    sum(){
        return num1+num2;
    }
}
var box=new Box(12,88);
console.log(box.hasOwnProperty("num1"));//true
console.log(box.hasOwnProperty("num2"));//true
console.log(box.hasOwnProperty("sum"));//false
console.log("num1" in box);//true
console.log("num2" in box);//true
console.log("sum" in box);//true
console.log("say" in box);//false
```
>类的所有实例共享一个原型对象，它们的原型都是Person.prototype，所以proto属性是相等的

### 模块化
ES6的模块化的基本规则和特点：  
1：每一个模块只加载一次， 每一个JS只执行一次， 如果下次再去加载同目录下同文件，直接从内存中读取。 一个模块就是一个单例，或者说就是一个对象；  
2：每一个模块内声明的变量都是局部变量， 不会污染全局作用域；  
3：模块内部的变量或者函数可以通过export导出；  
4：一个模块可以导入别的模块。  
在ES6中每一个模块即是一个文件，在文件中定义的变量，函数，对象在外部是无法获取的。如果你希望外部可以读取模块当中的内容，就必须使用export来对其进行暴露（输出）。
```javascript
let myName="laowang";
let myAge=90;
let myfn=function(){
    return "我是"+myName+"！今年"+myAge+"岁了"
}
//如果你不想暴露模块当中的变量名字，可以通过as来进行操作
export {
    myName,
    myAge as age,
    myfn as fn
}

/******************接收的代码调整为**********************/
import {fn,age,myName} from "./test.js";
console.log(fn());     //我是laowang！今年90岁了
console.log(age);      //90
console.log(myName);   //laowang

import * as info from "./test.js";  //通过*来批量接收，as 来指定接收的名字
console.log(info.fn());             //我是laowang！今年90岁了
console.log(info.age);              //90
console.log(info.name);             //laowang
```
### 箭头（Arrow）函数
这是ES6中最令人激动的特性之一。 => 不只是关键字function的简写，它还带来了其它好处。  
箭头函数与包围它的代码共享同一个 this,能帮你很好的解决this的指向问题。  
有经验的JavaScript开发者都熟悉诸如 var self = this; 或 var that =this这种引用外围this的模式,但借助 =>，就不需要这种模式了。  
* 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
* 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
* 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
* 不可以使用yield命令，因此箭头函数不能用作Generator函数。
```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};

//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。
var getTempItem = id => ({ id: id, name: "Temp" });
```

### 字符串模板
ES6支持 模板字符串，使得字符串的拼接更加的简洁、直观。
```javascript
//不使用模板字符串：
var name = 'Your name is ' + first + ' ' + last + '.'
//使用模板字符串：
var name = `Your name is ${first} ${last}.`
```
* 在ES6中通过 ${}就可以完成字符串的拼接，只需要将变量放在大括号之中。
* 在``中还可以随意折行

### 解构赋值和延展操作符
在ES5中，开发者们为了从对象和数组中获取特定数据并赋值给变量，编写了许多看起来同质化的代码，  
如果要提取更多变量，则必须依次编写类似的代码来为变量赋值，  
如果其中还包含嵌套结构，只靠遍历是找不到真实信息的，必须要深入挖掘整个数据结构才能找到所需数据，  
所以ES6添加了解构功能，将数据结构打散的过程变得更加简单，可以从打散后更小的部分中获取所需信息。  

#### 对象解构
```javascript
let node = {
    type: "Identifier",
    name: "foo"
};
let { type, name, value = true, key } = node;
console.log(type);       // "Identifier"
console.log(name);       // "foo"
console.log(value);      // true
console.log(key);        // undefined

//为非同名局部变量赋值
let { type: localType, name: localName } = node;
console.log(localType);   // "Identifier"
console.log(localName);   // "foo"
```
嵌套对象解构：解构嵌套对象仍然与对象字面量的语法相似，可以将对象拆解以获取想要的信息。

#### 数组解构
```javascript
let colors = [ "red", "green", "blue" ];
let [ firstColor, secondColor ] = colors;
console.log(firstColor);     // "red"
console.log(secondColor);    // "green"

//可以直接省略元素，只为感兴趣的元素提供变量名
let [ , , thirdColor ] = colors;
console.log(thirdColor);     // "blue"

// 在 ES6 中互换值
let a = 1,
    b = 2;
[ a, b ] = [ b, a ];
console.log(a);          // 2
console.log(b);          // 1

//在数组中，可以通过...语法将数组中的其余元素赋值给一个特定的变量
let colors = [ "red", "green", "blue" ];
let [ firstColor, ...restColors ] = colors;
console.log(firstColor); // "red"
console.log(restColors.length);   // 2
console.log(restColors[0]);       // "green"
console.log(restColors[1]);       // "blue"

// 在 ES6 中克隆数组
let colors = [ "red", "green", "blue" ];
let [ ...clonedColors ] = colors;
console.log(clonedColors);       //"[red,green,blue]"
```
#### 混合解构
```javascript
// options 上的属性表示附加参数
function setCookie(name, value, options) {
    options = options || {};
    let secure = options.secure,
        path = options.path,
        domain = options.domain,
        expires = options.expires;
        // 设置 cookie 的代码
}
// 第三个参数映射到 options
setCookie("type", "js", {
    secure: true,
    expires: 60000
});

//字符串解构
const [a, b, c, d, e] = 'hello';
console.log(a);           //"h"
console.log(b);           //"e"
console.log(c);           //"l"
console.log(d);           //"l"
console.log(e);           //"o"

const {length} = 'hello';
console.log(length);      //5
```
### 对象属性简写
ES6 允许在对象之中，只写属性名，不写属性值。
这时，属性值等于属性名所代表的变量。
并且方法也可以简写。
```javascript
var birth = '2000/01/01';
var Person = {
	name: ' 张三 ',
	// 等同于 birth: birth
	birth,
	//  等同于 hello: function ()...
	hello() { console.log(' 我的名字是 ', this.name); 
}

function getPoint() {
	var x = 1;
	var y = 10;
	return {x, y};
}
getPoint()       // {x:1, y:10}
```

### let和const
* let与const都是只在声明所在的块级作用域内有效。
* let不像var那样会发生“变量提升”现象。所以，变量一定要在声明后使用，否则报错。
* let声明的变量可以改变，值和类型都可以改变，没有限制。
* const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

### Promise
Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6将其写进了语言标准，统一了用法，原生提供了Promise对象。

