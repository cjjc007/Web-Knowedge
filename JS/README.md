### 内置类型
#### JS中原始类型有哪几种？null 是对象吗？原始数据类型和复杂数据类型有什么区别？
目前，JS原始类型有六种，分别为:  
Boolean  
String  
Number  
Undefined  
Null  
Symbol(ES6新增)  
ES10新增了一种基本数据类型：BigInt  
  
复杂数据类型只有一种: Object  
  
null 不是一个对象，尽管 typeofnull 输出的是 object，这是一个历史遗留问题，JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象， null 表示为全零，所以将它错误的判断为 object 。
