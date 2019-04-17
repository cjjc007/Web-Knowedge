## ES6新特性（2015）
ES6中的特性比较多，在这里列举常用的：
* 类
* 模块化
* 箭头函数
* 函数参数默认值
* 模板字符串
* 解构赋值
* 延展操作符
* 对象属性简写
* Promise
* Let与Const

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

