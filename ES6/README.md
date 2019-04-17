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
