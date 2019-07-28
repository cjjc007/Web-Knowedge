## 常见设计模式
### 什么是设计模式
设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。  
使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。  

### 单体模式
单体是一个用来划分命名空间并将一批相关的属性和方法组织在一起的对象，如果他可以被实例化，那么他只能被实例化一次。  
特点：　
* 可以来划分命名空间，从而清除全局变量所带来的危险。
* 利用分支技术来来封装浏览器之间的差异。
* 可以把代码组织的更为一体，便于阅读和维护。
代码实现：
```javascript
var Singleton = {
    attribute: true,
    method1: function(){},
　  method2: function(){}
};
```
应用场景：  
单体模式在我们平时的应用中用的比较多的，相当于把我们的代码封装在一个起来，只是暴露一个入口，从而避免全部变量的污染。  

### 工厂模式
提供创建对象的接口，因为创建对象可能会导致大量的重复代码，也可能提供不了足够级别的抽象。所以工厂就是把成员对象的创建工作转交给一个外部对象，好处在于消除对象之间的耦合(也就是相互影响)。  
* 简单工厂模式：使用一个类，通常为单体，来生成实例。
```javascript
unction CreatePerson(name,age,sex) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function(){
        return this.name;
    }
    return obj;
}
var p1 = new CreatePerson("longen",'28','男');
var p2 = new CreatePerson("tugenhua",'27','女');
console.log(p1.name); // longen
console.log(p1.age);  // 28
console.log(p1.sex);  // 男
console.log(p1.sayName()); // longen
 
console.log(p2.name);  // tugenhua
console.log(p2.age);   // 27
console.log(p2.sex);   // 女
console.log(p2.sayName()); // tugenhua
 
// 返回都是object 无法识别对象的类型 不知道他们是哪个对象的实列
console.log(typeof p1);  // object
console.log(typeof p2);  // object
console.log(p1 instanceof Object); // true
```
如上代码：函数CreatePerson能接受三个参数name,age,sex等参数，可以无数次调用这个函数，每次返回都会包含三个属性和一个方法的对象。
  
工厂模式是为了解决多个类似对象声明的问题;也就是为了解决实列化对象产生重复的问题。  
优点：能解决多个相似的问题。  
缺点：不能知道对象识别的问题(对象的类型不知道)。  

* 复杂工厂模式定义是：将其成员对象的实列化推到子类中，子类可以重写父类接口方法以便创建的时候指定自己的对象类型。  
父类只对创建过程中的一般性问题进行处理，这些处理会被子类继承，子类之间是相互独立的，具体的业务逻辑会放在子类中进行编写。  

### 单例模式
### 观察者模式（发布订阅模式）
### 策略模式
### 模板模式
### 代理模式
### 外观模式
