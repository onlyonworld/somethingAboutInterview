### 1、行内元素有哪些?  块级元素有哪些?   CSS的盒模型?

行内元素：a、em、i、big、strong、input、label、span等等

块级元素：div、form、h标签、hr、ul、ol、table等等

css的盒模型(四部分)：内容(content)、内边距（padding）、边框（border）、外边距（margin）

设置盒子模型：box-sizing：content-box | border-box，默认值：content-box

content-box：

padding和border不被包含在定义的width和height之内，即设置的宽高只是content的宽高。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding )

此属性表现为标准模式下的盒模型。

border-box：

padding和border被包含在定义的width和height之内，即设置的宽高包括了content、padding、border。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width )

### 2、CSS引入的方式有哪些? link和@import的区别是?

css引入方式：行内样式(内联方式)、内部样式(嵌入方式)、外部样式(链接方式)

link和@import的区别：

①link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载；

②link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器(IE5及以下)不支持；

③@import可以在CSS中再次引入其他样式表

④link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等，@import就只能加载CSS了。

### 3、CSS选择符有哪些?  哪些属性可以继承?  优先级算法如何计算?  内联和important哪个优先级高?

CSS选择符(器)有：id选择器(`#id`)、元素选择器(`dom`)、类选择器(`.class`)、子元素选择器(`div1 > div2`)、后代选择器(`div1 div2`)、相邻选择器(`div1 + div2`)、通配符选择器(`*`)、伪类选择器(`a:hover`)、属性选择器(`div[class="xxx"]`)

可以继承的属性：font、font-size、font-weight、text-align、line-height、color等等

 优先级算法计算：

​	1.优先级就近原则，同权重情况下样式定义最近者为准;
​	2.载入样式以最后载入的定位为准;
​	3.!important > id > class > tag;
​	4.!important比内联优先级高，但内联比id要高;

内联和important相比，important优先级高

### 4、前端页面有哪三三层构成，分别是什么?作用是什么?

三层构成：结构层(HTML)、样式层(CSS)、行为层(JavaScript)。

作用：

​	结构层(HTML)：用于存储客户想要阅读或查看的所有内容，包括文字，图片，视频等等

​	样式层(CSS)：决定了结构层中的数据以何种形式展现，提高用户视觉体验。

​	行为层(JavaScript)：使得页面具有交互性，允许页面响应用户操作或基于一组条件进行更改

### 5、如何居中一个元素?

#### 居中浮动元素的两种方法：

第一种方法是在需要居中的元素外面加一层div，然后给该div一个width，并设置其margin为auto，浮动元素的width设置为100%。

第二种方法是给元素一个宽度width，同时设置元素的margin-left的值为50%，使用相对定位(position: relative)，left的值为该元素宽度的负一半(left: -50%width)

#### 居中普通div元素的3种方法

第一种是使用flex布局，通过 `align-items: center`和`justify-content: center`设置容器的垂直和水平方向上为居中对齐，然后它的子元素也可以实现垂直和水平的居中。

第二种方法是div元素的宽高固定，利用绝对定位，先将元素的左上角通过`top: 50%`和 `left: 50%`定位到页面的中心，然后再通过`margin: -50%height -50%width`负值来调整元素的中心点到页面的中心。

第三种是利用绝对定位，先将元素的左上角通过`top: 50%`和`left: 50%`定位到页面的中心，然后再 通过`transform: translate(-50%, -50%)`来调整元素的中心点到页面的中心。

### 6、HTML5和css3新特性

#### HTML5：

文档声明简化了

```html
html5：
	<！DOCTYPE html>
html5之前：
	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
```

新增/废除一些标签：

​	新增：语义化标签：header、footer、nav、article、aside、section、canvas、progress、meter

​				属性：input新增email、date、datetime、tel、

​	废除：frame、frameset、big、font

​				属性：link标签和a标签中的charset、link中的target、div、h标签、intput标签和p标签的align

#### CSS3：

过渡效果transition

形状转换transform

盒子阴影box-shadow

图像边框border-image

文字换行

​	word-break：normal|break-all|keep-all; 

​	word-wrap: normal|break-word; 

​	超出省略号  text-overflow:clip|ellipsis|string 

颜色rbga：a是不透明度

布局：弹性布局flex、栅格布局grid

盒模型定义：

​	 box-sizing:border-box的时候，边框和padding包含在元素的宽高之内！              

​	box-sizing:content-box的时候，边框和padding不包含在元素的宽高之内！

### 7、css样式为何放前面，js文件为何放后面

​		如果将css放在头部，css的下载解析是可以和html的解析同步进行的，放到尾部，要花费额外时间来解析CSS，并且浏览器会先渲染出一个没有样式的页面，等CSS加载完后会再渲染成一个有样式的页面，页面会出现明显的闪动的现象。

​		因为当浏览器解析到script的时候，就会立即下载执行，中断html的解析过程，如果外部脚本加载时间很长（比如一直无法完成下载），就会造成网页长时间失去响应，浏览器就会呈现“假死”状态，这被称为“阻塞效应”。

### 8、display:none，visibility:hidden，opactiy:0的区别

display:none隐藏后不占据额外空间，它会产生回流和重绘，而visibility:hidden和opacity:0元素虽然隐藏了，但它们仍然占据着空间，它们俩只会引起页面重绘。

display:none不会被子元素继承；visibility:hidden 会被子元素继承，可以通过设置子元素visibility:visible 使子元素显示出来；opacity: 0 也会被子元素继承，但是不能通过设置子元素opacity: 1使其重新显示

对于过渡效果，只对opacity有效，对其他两个无效。

### 9、 padding , margin 百分比单位依据

容器的宽度：width

### 10、什么是BFC

BFC (Block formatting context)：块级格式化格式化上下文，它是一个独立的渲染区域，只有Block-level box(块级盒)参与， 它规定了内部的Block-level Box(块级盒)如何布局，并且与这个区域外部毫不相干。

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

#### 创建BFC

- 1、float的值不是none。

- 2、position的值不是static或者relative。

- 3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex

- 4、overflow的值不是visible

### 11、绝对定位元素 与 非绝对定位元素的百分比计算的区别

绝对定位元素的宽高百分比是相对于临近的 position 不为 static 的祖先元素的 paddingbox 来 计算的。 

```css
.box1 {
	width: 100px;
    height: 100px;
    background-color: blueviolet;
    padding: 25px;
    position: relative;
}
.box2 {
	position: absolute;
    width: 50%;
    height: 50%;
    background-color: cyan;
}
<div class="box1">
	<div class="box2"></div>
</div>
```

<img src="D:\typora笔记\面试.assets\image-20201113114051651.png" alt="image-20201113114051651" style="zoom:67%;" />

```css
.box3 {
    width: 100px;
    height: 100px;
    background-color: blueviolet;
    position: relative;
}
.box4 {
    position: static;
    width: 80px;
    height: 80px;
    background-color: blue;
}
.box5 {
    position: absolute;
    height: 50%;
    width: 50%;
    background-color: cyan;
}
<div class="box3">
    <div class="box4">
        <div class="box5"></div>
    </div>
</div>
```

![image-20201113114427717](D:\GitHubRepository\html+css篇.assets\image-20201113114427717-1607739771810.png)

非绝对定位元素的宽高百分比则是相对于父元素的 contentbox 来计算的。

```css
.box6 {
    height: 120px;
    width: 120px;
    background-color: darkblue;
}
.box7 {
    width: 50%;
    height: 50%;
    background-color: darkorchid;
}
<div class="box6">
    <div class="box7"></div>
</div>
```

<img src="D:\typora笔记\面试.assets\image-20201113114640323.png" alt="image-20201113114640323" style="zoom:67%;" />

### 12、margin重合问题

① 相邻兄弟元素的`marin-bottom`和`margin-top`的值发生重叠。这种情况下我们可以通过设置其中一个元素为`BFC`来解决。

② 父元素的`margin-top`和子元素的`margin-top`发生重叠。它们发生重叠是因为它们是相邻的，所以我们可以通过这 一点来解决这个问题。

③ 高度为`auto`的父元素的`margin-bottom`和子元素的`margin-bottom`发生重叠。它们 发生重叠一个是因为它们相邻，一个是因为父元素的高度不固定。因此我们可以为父元素设置`border-bottom、 padding-bottom`来分隔它们，也可以为父元素设置一个高度，`max-height`和`min-height`也能解决这个问题。

④ 没有内容的元素，自身的`margin-top`和`margin-bottom`发生的重叠。我们可以通过为其设置 `border`、`padding`或者高度来解决这个问题

### 13、overflow:scroll; 时不能平滑滚动的问题怎么处理？

以下代码可解决这种卡顿的问题：`-webkit-overflow-scrolling: touch;`

是因为这行代码启用了硬 件加速特性，所以滑动很流畅。

### 14、如何解决border重叠问题

边框重叠可以分为两种情况，分别为：

1、div，ul等元素盒子设置边框后的重叠问题

```css
border: 1px solid #000000;
margin:0px -1px -1px 0px ;
```

2、table表格设置边框后的重叠问题

```css
table {
	border-collapse: collapse;
}
table td {         
    border: 1px solid #000000;
    padding: 20px 30px;
}
```

### 15、float的工作原理

浮动元素是脱离文档流的，不占据空间。

浮动的元素会尽量向左 / 右移动，直到它的外边缘遇到包含框或者另一个浮动框的边缘为止

### 16、如何去除img元素底部的空白

css中对于`display: inline`元素的`vertical-align`的默认值是`baseline`，用一张图来观察`vertical-align`各个值之间的差别，可以看出`baseline`和`bottom`之间有一定的距离

<img src="D:\typora笔记\面试.assets\image-20201119133622961.png" alt="image-20201119133622961" style="zoom:67%;" />

`img`标签默认就是一个行内元素，所以图片下面那一道空白正是`baseline`和`bottom`之间的这段距离。即使只有图片没有文字，只要是`inline`的图片这段空白都会存在。要去掉这段空白，最直接的办法是将图片的`vertical-align`设置为其他值。如果在同一行里有文字混排的话，那应该是用`bottom`或是`middle`比较好。

另外，`top`和`bottom`之间的值即为`line-height`。假如把`line-height`设置为0，那么`baseline`与`bottom`之间的距离也变为0，那道空白也就不见了；在没有设置`line-height`的情况下把`font-size`设为0也可以达到同样的效果。当然，这样做的后果就是不能图文混排了。

### 17、两栏布局、三栏布局

**两栏布局一般指的是页面中一共两栏，左边固定，右边自适应的布局**

(1) 利用浮动，将左边元素宽度设置为 200px，并且设置向左浮动。将右边元素的 margin-left 设置为 200px，宽度设置为 auto（默认为 auto，撑满整个父元素）。

(2) 利用 flex 布局，将左边元素的放大和缩小比例设置为 0，基础大小设置为 200px。将右边的元素的放大比例设置为 1，缩小比例设置为 1，基础大小设置为 auto。

(3) 利用绝对定位布局的方式，将父级元素设置相对定位。左边元素设置为 absolute 定位，并且宽度设置为 200px。将右边元素的 margin-left 的值设置为 200px。

(4) 利用绝对定位的方式，将父级元素设置为相对定位。左边元素宽度设置为 200px，右边元素设置为绝对定位，左边定位为 200px，其余方向定位为 0。

**三栏布局一般指的是页面中一共有三栏，左右两栏宽度固定，中间自适应的布局**

(1) 利用绝对定位的方式，左右两栏设置为绝对定位，中间设置对应方向大小的 margin 的 值。

(2) 利用 flex 布局的方式，左右两栏的放大和缩小比例都设置为 0，基础大小设置为固定 的大小，中间一栏设置为 auto。

(3) 利用浮动的方式，左右两栏设置固定大小，并设置对应方向的浮动。中间一栏设置左右两个方向的 margin 值，注意这种方式，中间一栏必须放到最后。

(4) **圣杯布局**，利用浮动和负边距来实现。**父级元素**设置左右的 padding，三列均设置向左 浮动，中间一列放在最前面，宽度设置为父级元素的宽度，因此后面两列都被挤到了下一行， 通过设置 margin 负值将其移动到上一行，再利用相对定位，定位到两边。

(5) **双飞翼布局**，也是利用浮动和负边距来实现，左右位置的保留是通过**中间列**的 margin 值来实现的，而不是通过父元素的 padding 来实现的。

### 18、CSS如何保证类名不重复

`VUE`中提供了`scoped`，在style标签中加入`scoped`属性，相当于在元素中添加了一个唯一的属性来进行区分

`CSS Modules`通过**给样式名加hash字符串后缀**的方式，实现特定作用域语境中的样式编译后的样式在全局唯一。

### 19、canvas常用方法

```js
//绘制矩形，前两个参数代表矩形的起点，后两个参数代表矩形的宽高
canText2.rect(50,25,100,100);
//绘制圆形，arc(x,y,r,SAngle,eAngle,counterclockwise)方法创建弧/曲线
// x, y分别为圆的中心的横纵坐标
// r为圆的半径
// sAngle起始角，以弧度计。即多少π (弧的圆形的三点钟位置是0度)
// eAngle结束角，以弧度计。即多少π
// counterclockwise 选填项。规定顺时针还是逆时针绘图。false为顺时针, true为逆时针，默认是false
canText3.arc(75,75,70,0,1*Math.PI,false);
//需要填充的颜色
canText2.fillStyle="greenyellow";
//执行填充颜色的操作
canText2.fill();
//将绘制的图形展示出来
canText.stroke();
//先开始一段路径，声明图形绘制从这段程序开始
canText.beginPath();
//定义一个起点，即我们要绘制的图形的起点
canText.moveTo(50,50);
//添加1个或多个点并将初始点与该点连接起来
canText.lineTo(150,50);
 //关闭路径，表示绘制图形结束
canText.closePath();
```

### 20、css中两种动画：transition和animation

```js
语法：transition: property duration timing-function delay;

transition-property:指定CSS属性的name, 
transition-duration:需要指定多少秒或毫秒才能完成
transition-timing-function:指定transition效果的转速曲线
transition-delay:定义trinsition效果开始的时候，即延时多少时间开始过渡效果

语法: animation:  name duration timing-function delay iteration-count direction fill-mode   play-state;

animation-name：指定动画特效的名称
animation-duration：动画特效需要多少秒或毫秒完成
animation-timing-function：设置动画特效将如何完成一个周期，效果路线
animation-delay：设置动画特效在启动前的延迟间隔。
animation-iteration-count：定义动画特效的播放次数。
animation-direction：指定是否应该轮流反向播放动画特效。
animation-fill-mode：规定当动画特效不播放时(当动画完成时，或当动画有一个延迟未开始播放时)，要应用到元素的样式。
animation-play-state：指定动画特效是否正在运行或已暂停。
```

### 21、font-face是什么

font-face是css3中允许使用自定义字体的一个模块。

用法：`@font-face{font-family: myFirstFont; src: url('Sansation_Light.ttf'), url('Sansation_Light.eot'); /* IE9 */}`

### 22、rem和em的区别

rem是基于html元素的字体大小来决定，而em则根据使用它的元素的大小决定。

二者都是灵活、可扩展的单位，由浏览器转换为像素值，具体取决于您的设计中的字体大小设置。

### 23、HTML5自定义属性

在HTML5中添加了`data-*`的方式来自定义属性，所谓`data-*`实际上上就是`data-`前缀加上自定义的属性名，使用这样的结构可以进行数据存放。使用`data-*`可以解决自定义属性混乱无管理的现状。

```html
<!-- data-*有两种设置方式，可以直接在HTML元素标签上书写 -->
<div id="test" data-age="24"> hello world </div>

<script>
    // 这样就为div添加了一个data-my的自定义属性
	var test = document.getElementById('test');
	test.dataset.my = 'first';
    test.dataset.birthDate = '134567097';
</script>

使用JavaScript操作dataset有两个需要注意的地方:
1. 我们在添加或读取属性的时候需要去掉前缀data-*
2. 如果属性名称中还包含连字符(-)，需要转成驼峰命名方式，但如果在CSS中使用选择器，我们需要使用连字符格式
<style type="text/css">
	[data-birth-date] {
		background-color: #0f0;
		width:100px;
		margin:20px;
	}
</style>
```

### 24、padding和margin百分比的计算

无论是`padding-left / padding-right / padding-top / padding-bottom / margin-left / margin-right / margin-top / margin-bottom`，都是相对于父元素的宽度来计算的











