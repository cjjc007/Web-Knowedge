* css书写规范
* 盒模型
* 单位px/rem/em
* BFC
* 隐藏元素
* css3选择器
* 三栏布局
* 垂直居中
* position:
* display:
* 浮动和清除浮动
* css3新增样式

## css书写规范
#### CSS书写顺序:  
1.位置属性(position, top, right, z-index,display, float等)  
2.大小(width, height, padding, margin)  
3.文字系列(font, line-height, letter-spacing,color- text-align等)  
4.背景(background, border等)  
5.其他(animation, transition等)  
#### 注意事项:
* 一律小写，中划线  
* 尽量不用缩写  
* 不要随便使用id  
* 去掉小数点前面的0： 0.9rem => .9rem  
* 使用简写：margin： 0 1rem 3rem  

## 盒模型
可以说，页面就是由一个个盒模型堆砌起来的，每个HTML元素都可以叫做盒模型，盒模型由外而内包括：边距（margin）、边框（border）、填充（padding）、内容（content）。  
它在页面中所占的实际宽度是margin + border + padding + content 的宽度相加。  
#### 盒模型有标准盒模型和IE的盒模型:  
标准盒模型的内容大小就是content的大小  
IE盒模型的内容则是content + padding +border 总的大小  
#### 设置：
box-sizing: content-box(W3C)/border-box(IE)  

## 单位px/rem/em
#### px ：像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。
px 特点：  
1. IE无法调整那些使用px作为单位的字体大小；  
2. 国外的大部分网站能够调整的原因在于其使用了em或rem作为字体单位；  
3. Firefox能够调整px和em，rem，但是96%以上的中国网民使用IE浏览器(或内核)。  
  
#### em ：是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。
em 特点：  
1. em的值并不是固定的；  
2. em会继承父级元素的字体大小。  
  
任意浏览器的默认字体高都是16px。所有未经调整的浏览器都符合: 1em=16px。那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明Font-size=62.5%，这就使em值变为 16px*62.5%=10px,这样12px=1.2em, 10px=1em,也就是说只需要将你的原来的px数值除以10，然后换上em作为单位就行了。  

#### rem ：是CSS3新增的一个相对单位（root em，根em），这个单位引起了广泛关注。
这个单位与em有什么区别呢？  
区别在于使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。  
目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器，应对方法也很简单，就是多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小。  

## BFC
BFC 是 Block Formatting Context 的缩写，即块格式化上下文。  
#### BFC布局规则
* BFC内，盒子依次垂直排列。
* BFC内，两个盒子的垂直距离由 margin 属性决定。属于同一个BFC的两个相邻Box的margin会发生重叠【符合合并原则的margin合并后是使用大的margin】
* BFC内，每个盒子的左外边缘接触内部盒子的左边缘（对于从右到左的格式，右边缘接触）。即使在存在浮动的情况下也是如此。除非创建新的BFC。
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
* 计算BFC的高度时，浮动元素也参与计算。

#### 如何创建BFC
* 根元素
* 浮动元素（float 属性不为 none）
* position 为 absolute 或 relative
* overflow 不为 visible 的块元素
* display 为 inline-block, table-cell, table-caption

#### BFC的应用
1.防止 margin 重叠  
当两个div的 margin 都设置50px，会发生 margin 的重叠。  
根据BFC规则，同一个BFC内的两个两个相邻Box的 margin 会发生重叠，因此我们可以在div外面再嵌套一层容器，并且触发该容器生成一个 BFC，这样 <divclass="a"></div> 就会属于两个 BFC，自然也就不会再发生 margin 重叠。  
```css
<style>
.a{
  height: 100px;
  width: 100px;
  margin: 50px;
  background: pink;
}
.container{
  overflow: auto; /*触发生成BFC*/
}
</style>
<body>
  <div class="container">
    <div class="a"></div>
  </div> 
<div class="a"></div>
</body>
```
2.清除内部浮动  
如果我们希望 container 的高度能够包含浮动元素，那么可以创建一个新的 BFC，因为根据 BFC 的规则，计算 BFC 的高度时，浮动元素也参与计算。
```css
<style>
.a{
  height: 100px;
  width: 100px;
  margin: 10px;
  background: pink;
  float: left;
}
.container{
  width: 120px;
  display: inline-block;/*触发生成BFC*/
  border: 2px solid black; 
}
</style>
```
3.自适应多栏布局  
根据规则，BFC的区域不会与float box重叠。因此，可以触发生成一个新的BFC。
```css
<style>
body{
  width: 500px;
}
.a{
  height: 150px;
  width: 100px;
  background: pink;
  float: left;
}
.b{
  height: 200px;
  background: blue;
  overflow: hidden; /*触发生成BFC*/
}
</style>
<body>
  <div class="a"></div>
  <div class="b"></div>
</body> 
```

## 隐藏元素
#### 隐藏类型：  
屏幕并不是唯一的输出机制，比如说屏幕上看不见的元素（隐藏的元素），其中一些依然能够被读屏软件阅读出来（因为读屏软件依赖于可访问性树来阐述）。为了消除它们之间的歧义，我们将其归为三大类：  
* 完全隐藏：元素从渲染树中消失，不占据空间。  
* 视觉上的隐藏：屏幕中不可见，占据空间。  
* 语义上的隐藏：读屏软件不可读，但正常占据空。  

#### 完全隐藏
1. display 属性(不占据空间)
```css
display: none;
```
2.hidden 属性 (不占据空间)
HTML5 新增属性，相当于 display:none
```css
<div hidden></div>
```
#### 视觉上的隐藏
1.利用 position 和 盒模型 将元素移出可视区范围  
* 设置 posoition 为 absolute 或 fixed，通过设置 top、 left 等值，将其移出可视区域。(可视区域不占位)  
```css
position:absolute;  
left: -99999px;  
```
* 设置 position 为 relative，通过设置 top、 left等值，将其移出可视区域。（可视区域占位）
```css
position: relative;  
left: -99999px;  
// 如希望其在可视区域不占位置，需同时设置 height:0; 
```
  
* 设置 margin 值，将其移出可视区域范围（可视区域占位）。
```css
margin-left: -99999px;  
// 如果希望其在可视区域不占位，需同时设置 height:0;  
```  
2.利用 transfrom  
* 缩放(占据空间)  
```css
transform: scale(0);  
//如果希望不占据空间，需同时设置 height:0  
```
  
* 移动 translateX, translateY(占据空间)
```css
transform: translateX(-99999px);  
// 如果希望不占据空间，需同时设置 height:0  
```
* 旋转 rotate (占据空间)  
```css
transform: rotateY(90deg);  
```
  
3.设置其大小为0  
* 宽高为0，字体大小为0：  
```css
height: 0;  
width: 0;  
font-size: 0;  
```
  
* 宽高为0，超出隐藏:  
```css
height: 0;
width: 0;
overflow: hidden;
```
  
4.设置透明度为0 (占据空间)  
```css
opacity: 0;
```
  
5. visibility属性 (占据空间)  
```css
visibility: hidden
```
  
6.层级覆盖， z-index 属性 (占据空间)  
```css
position: relative;
z-index: -999;
// 再设置一个层级较高的元素覆盖在此元素上
```
  
7.clip-path 裁剪 (占据空间)  
```css
clip-path: polygon(0 0, 0 0, 0 0, 0 0);
```
#### 语义上的隐藏
aria-hidden 属性 (占据空间)  
读屏软件不可读，占据空间，可见。  
```css
<div aria-hidden="true">
</div>
```


## css3选择器
## 三栏布局
## 垂直居中
## position:
## display:
## 浮动和清除浮动
## css3新增样式
