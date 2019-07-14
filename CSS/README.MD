* css书写规范
* 盒模型
* 单位px/rem/em
* BFC
* 隐藏元素
* css选择器
* 三栏布局
* 垂直居中
* position:
* display:
* 浮动和清除浮动
* css3新增样式
* canvas
* Animation

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

## css选择器
* 标签选择
* id选择器
* class选择器
* 后代选择 （div a）
* 子代选择 （div > p）
* 相邻选择 （div + p）
* 通配符选择 （*）
* 否定选择器 :not(.link){}
* 属性选择器
* 伪类选择器
* 伪元素选择器 ::before{}

#### 属性选择器:
E[attr] 属性名，不确定具体属性值  
E[attr=“value”] 指定属性名，并指定其对应属性值  
E[attr ~=“value”] 指定属性名，找到的是具有此属性名，且与其它属性名之间用空格隔开  
E[attr ^= “value”] 指定属性名，属性值以value开头  
E[attr $=“value”] 指定属性名，属性值以value结束  
E[attr *=“value”] 指定了属性名，属性值中包含了value  
E[attr |= “value”] 指定属性名，属性值以value-开头或者值为value  
  
#### 伪元素选择器：
::first-line 匹配文本块的首行  
::first-letter 选择文本块的首字母  
  
#### 伪类选择器：
:before, :after在元素内容前面、后面添加内容(相当于行内元素)  
:link指向未被访问页面的链接设置样式  
:visited设置指向已访问页面的链接的样式  
:hover鼠标悬停时触发  
:active在点击时触发  
:enabled 选择启用状态元素  
:disabled 选择禁用状态元素  
:checked 选择被选中的input元素（单选按钮或复选框）  
  
#### CSS3结构选择器
:nth-child 选择指定索引处的子元素  
nth-child(n) 父元素下的第n个子元素  
nth-child(odd) 奇数子元素(同nth-child(2n-1))  
nth-child(even) 偶数子元素(同nth-child(2n))  
nth-child(an+b) 公式  
(nth-child从1开始)  
:nth-last-child(n) 倒数第n个子元素  
:nth-of-type(n) 父元素下的第n个指定类型的子元素  
:nth-last-of-type 父元素下的倒数第n个指定类型的子元素  
:first-child 选择父元素下的第一个子元素  
:last-child 选择父元素下的最后一个子元素  
:only-child 选择父元素下唯一的子元素  
:only-of-type	选择父元素下指定类型的唯一子元素  
  

## 三栏布局
实现效果： 左右栏定宽，中间栏自适应
#### 1、绝对定位布局：position + margin
html结构：
```html
    <div class="container">
        <div class="left">Left</div>
        <div class="right">Right</div>
        <div class="main">Main</div>
    </div>
```
css样式：
```css
     body,html{
        height: 100%;
        padding: 0;
        margin: 0;
        overflow: hidden;
    }
    /*左右进行绝对定位*/
    .left,.right{
        position: absolute;
        height:100%;  
        top: 0;
        background: #ff69b4;
    }
    .left{
        left: 0;
        width: 100px;
    }
    .right{
        right: 0;
        width: 200px;
    }
    /*中间用margin空出左右元素所占的空间*/
    .main{
        height:100%; 
        margin: 0 100px 200px 0;
        background: #659;
    }
```
缺点: 如果中间栏含有最小宽度限制，或是含有宽度的内部元素，当浏览器宽度小到一定程度，会发生层重叠的情况。

#### 2、浮动布局： float + margin
html结构：
```html
    <div class="container">
        <div class="left">Left</div>
        <div class="right">Right</div>
        <div class="main">Main</div>
    </div>
```
css样式：
```css
        body,html{
            height: 100%;
            padding:0;
            margin: 0;
        }
        /*左边栏左浮动*/
        .left{
            float:left;
            height:100%;
            width:100px;
            background:#ff69b4;
        }
        /*中间栏自适应*/
        .main{
            height:100%;
            margin:0 200px 0 100px;
            background: #659;
        }
        /*右边栏右浮动*/
        .right{
            float:right;
            height:100%;
            width:200px;
            background:#ff69b4;
        }
```
#### 3、flex布局
html结构：
```html
    <div class="container">
        <div class="left"></div>
        <div class="main">center</div>
        <div class="right">right</div>
    </div>
```
css样式：
```css
     .container{
            display: flex;
        }
        .left{
            width:200px;
            background: red;
        }
        .main{
            flex: 1;
            background: blue;
        }
        .right{
            width:200px;
            background: red;
        }
```
这种布局方式，高度由内容决定。
#### 4、table布局
html结构:
```html
    <div class="container">
        <div class="left">left</div>
        <div class="main">center</div>
        <div class="right">right</div>
    </div>
```
css样式：
```css
         .container{
            display: table;
            width:100%;
        }
        .container>div{
            display: table-cell;
        }
        .left{
            width: 100px;
            background: red;
        }
        .main{
            background: blue;
        }
        .right{
            width: 200px;
            background: red;
        }
```
高度由内容决定。

#### 5、Grid网格布局
html结构：
```html
    <div class="container">
        <div class="left">left</div>
        <div class="main">center</div>
        <div class="right">right</div>
    </div>
```
css样式：
```css
        .container{
            display: grid;
            width: 100%;
            grid-template-rows: 100px;  /*设置行高*/
            grid-template-columns: 100px auto 200px;  /*设置列数属性*/
        }
        .left{
            background: red;
        }
        .main{
            background: blue;
        }
        .right{
            background:red;
        }
```
#### 6、圣杯布局
html结构：
```html
    <div class="container">
        <div class="main col">Main</div>
        <div class="left col">Left</div>
        <div class="right col">Right</div>
    </div>
```
css样式：
```css
        /* 两边定宽，中间自适用 */
        body,html,.container{
            height: 100%;
            padding:0;
            margin: 0;
        }
        .col{
            float: left;   /* 三个col都设置float: left,为了把left和right定位到左右部分 */
            position:relative;
        }
         /*父元素空出左右栏位子: 因为上一步中，左右栏定位成功了，但是中间栏的内容会被遮盖住*/
        .container{
            padding:0 200px 0 100px;
        }
        /*左边栏*/
        .left{
            left:-100px;
            width: 100px;
            height:100%;
            margin-left: -100%;
            background: #ff69b4;
        }
        /*中间栏*/
        .main{
            width:100%;
            height: 100%;
            background: #659;
        }
        /*右边栏*/
        .right{
            right:-200px;
            width:200px;
            height:100%;
            margin-left: -200px;
            background: #ff69b4;
        }
```
总结：圣杯布局用到了浮动float、负边距、相对定位relative，不添加额外标签

#### 7、双飞翼布局
html结构：
```html
    <div class="container">
        <div class="main col ">
            <div class="main_inner">Main</div>
        </div>
        <div class="left col ">Left</div>
        <div class="right col ">Right</div>
    </div>
```
css样式：
```css
        body,html,.container{
            height: 100%;
            padding:0;
            margin: 0;
        }
        .col{ 
            float: left; /* 把left和right定位到左右部分 */
        }
        .main{ 
            width:100%;
            height:100%;
            background: #659;
        }
        .main_inner{   /* 处理中间栏的内容被遮盖问题 */
            margin:0 200px 0 100px;
        }
        .left{
            width: 100px;
            height: 100%;
            margin-left: -100%;
            background: #ff69b4;
        }
        .right{
            height:100%;
            width:200px;
            margin-left: -200px;
            background: #ff69b4;
        }
```
#### 双飞翼布局的好处：
（1）主要的内容先加载的优化。  
（2）兼容目前所有的主流浏览器，包括IE6在内。  
（3）实现不同的布局方式，可以通过调整相关CSS属性即可实现。  
  
#### 8、对比圣杯布局和双飞翼布局：
（1）都是左右栏定宽，中间栏自适应的三栏布局，中间栏都放到文档流前面，保证先行渲染。  
（2）解决方案基本相似：都是三栏全部设置左浮动float：left,然后分别结局中间栏内容被覆盖的问题。  
（3）解决中间栏内容被覆盖问题时，圣杯布局设置父元素的padding,双飞翼布局在中间栏嵌套一个div,内容放到新的div中，并设置margin，实际上，双飞翼布局就是圣杯布局的改进方案。  

## 垂直居中
父元素  .container  
子元素  .box  

#### 1、利用 flex 布局
```css
/* 无需知道被居中元素的宽高 */
.container {
    display: flex;
    align-items: center;
    justify-content: center;
}
```
#### 2、子元素是单行文本
设置父元素的 text-align 和 line-height = height
```css
.container {
    height: 100px;
    line-height: 100px;
    text-align: center;
}
```
#### 3、利用 absolute + transform
```css
/* 无需知道被居中元素的宽高 */
/* 设置父元素非 `static` 定位 */
.container {
    position: relative;
}
/* 子元素绝对定位，使用 translate的好处是无需知道子元素的宽高 */
/* 如果知道宽高，也可以使用 margin 设置 */
.box {
    position: absolute;
    left: -50%;
    top: -50%;
    transform: translate(-50%, -50%);
}
```
#### 4、利用 grid 布局
```css
/* 无需知道被居中元素的宽高 */
.container {
    display: grid;
}
.box {
    justify-self: center; 
    align-self: center;
}
```
#### 5、利用绝对定位和 margin:auto
```css
/* 无需知道被居中元素的宽高 */
.box {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
.container {
    position: relative;
}
```

## position
（1）static  
static是position属性的默认值，默认情况下，块级元素和行内元素按照各自的特性进行显示。  
（2）relative  
relative相对定位，其相对于父元素进行偏移，设置了这个属性后，元素会根据top，left，bottom，right进行偏移，关键点是它原本的空间仍然保留。  
（3）absolute  
绝对定位, 元素设置成absolute后会脱离文档流，并且不占有原本的空间，后面的元素会顶替上去，而且不论元素是行内元素还是块级元素，都会生成一个块级框，也就是例如行内元素span设置了absolute后就可以设置height和width属性了。  
（4）fixed  
fixed的表现方式类似于absolute，但是相比于absolute相对于不确定的父元素进行偏移，fixed就是相对于浏览器窗口进行偏移。  

## display
1、none： 元素不会显示，而且改元素现实的空间也不会保留。但有另外一个 visibility: hidden， 是保留元素的空间的。  
2、inline： display的默认属性。将元素显示为内联元素，元素前后没有换行符。我们知道内联元素是无法设置宽高的，所以一旦将元素的display 属性设为 inline， 设置属性height和width是没有用的。此时影响它的高度一般是内部元素的高度（font-size）和padding。  
3、block： 将元素将显示为块级元素，元素前后会带有换行符。设置为block后，元素可以设置width和height了。元素独占一行。  
4、inline-block： 行内块元素。这个属性值融合了inline 和 block 的特性，即是它既是内联元素，又可以设置width和height。  
5、table： 此元素会作为块级表格来显示（类似 <table>），表格前后带有换行符。  
6、table-cell： 此元素会作为一个表格单元格显示（类似 <td> 和 <th>）  
7、inherit： 规定应该从父元素继承 display 属性的值。  
8、grid： 网格布局，每个网格都有起止线，每两条线之间都有设定好的距离。
  ```css
.parent {
    display: grid;
    grid-template-columns: 60px 180px;  //第一列60px,第二列180px
    grid-template-rows: 10px auto;      // 第一行10px,第二行自适应
    grid-auto-flow: dense;              // 依次填充每行，默认row,还有column
    .key {
        grid-column-start: 1;  // 正常情况是一行一行填补数据，现在把key设置为第一列数据，纵向排列
        grid-column-end: 2;
        justify-items: end;    // 这一列右对齐...start/center
    }
    .value {
        grid-column-start: 2;  // value设置为第二列数据，纵向排列
        grid-column-end: 3;
        word-wrap: wrap;
        word-break: break-word;
    }
 }
  ```
  
## 浮动和清除浮动
当容器的高度为auto，且容器的内容中有浮动（float为left或right）的元素，在这种情况下，容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象。这个现象叫浮动溢出，为了防止这个现象的出现而进行的CSS处理，就叫CSS清除浮动。  
```html
<style>
.inner {
  width: 100px;
  height: 100px;
  float: left;
}
</style>
  <div class='outer'>
  <div class='inner'></div>
  <div class='inner'></div>
  <div class='inner'></div>
</div>
```
#### 1. 利用 clear 属性
在 <divclass='outer'> 内创建一个空元素，对其设置 clear:both; 的样式。  
优点：简单，代码少，浏览器兼容性好。  
缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。  
#### 2. 利用 clear 属性 + 伪元素
```css
.outer:after{
    content: '';
    display: block;
    clear: both;
    visibility: hidden;
    height: 0;
}
// IE8以上和非IE浏览器才支持:after，如果想要支持IE6、7，需要给 outer 元素，设置样式 zoom:1
```
#### 3. 利用 BFC 布局规则
根据 BFC 的规则，计算 BFC 的高度时，浮动元素也参与计算。因此清除浮动，只需要触发一个BFC即可。  
可以使用以下方法来触发BFC  
position 为 absolute 或 fixed  
overflow 不为 visible 的块元素  
display 为 inline-block, table-cell, table-caption  
如：
```css
.outer {
    overflow: hidden;
}
```
注意使用 display:inline-block 会产生间隙。
## css3新增样式
## canvas
## nimation
