### 对设计模式的理解
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
