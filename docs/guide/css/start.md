# CSS基础

## css三大特性 **继承/优先级/层叠**

**继承：** 即子类元素继承父类的样式;

**优先级：** 是指不同类别样式的权重比较;

**层叠：** 是说当数量相同时，通过层叠(后者覆盖前者)的样式。

## css选择符分类　　

首先来看一下css选择符(css选择器)有哪些?

　　1.标签选择器(如：body,div,p,ul,li)  
  
　　2.类选择器(如：class="head",class="head_logo")  
  
　　3.ID选择器(如：id="name",id="name_txt")  
  
　　4.全局选择器(如：*号)  
  
　　5.组合选择器(如：.head .head_logo,注意两选择器用空格键分开)  
  
　　6.后代选择器 (如：#head .nav ul li 从父集到子孙集的选择器)  
  
　　7.群组选择器 div,span,img {color:Red} 即具有相同样式的标签分组显示  
  
　　8.继承选择器(如：div p,注意两选择器用空格键分开)  
  
　　9.伪类选择器(如：就是链接样式,a元素的伪类，4种不同的状态：link、visited、active、hover。)  
  
　　10.字符串匹配的属性选择符(^ $ *三种，分别对应开始、结尾、包含)  
  
　　11.子选择器 (如：div>p ,带大于号>)  
  
　　12.CSS 相邻兄弟选择器器 (如：h1+p,带加号+)

## css优先级

当两个规则都作用到了同一个html元素上时，如果定义的属性有冲突，那么应该用谁的值的，CSS有一套优先级的定义。

**不同级别**

1.  在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式。
2.  作为style属性写在元素内的样式
3.  id选择器
4.  类选择器
5.  标签选择器
6.  通配符选择器
7.  浏览器自定义或继承

      **总结排序：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性**

**同一级别**

同一级别中后写的会覆盖先写的样式

上面的级别还是很容易看懂的，但是有时候有些规则是多个级别的组合，像这样



（权值实际并不是按十进制，用数字表示只是说明思想，一万个class也不如一个id权值高）

-   内联样式表的权值为 1000
-   ID 选择器的权值为 100
-   Class 类选择器的权值为 10
-   HTML 标签选择器的权值为 1


### 另外一种理解方式：

　　CSS优先级：是由四个级别和各级别的出现次数决定的。  
  
　　四个级别分别为：行内选择符、ID选择符、类别选择符、元素选择符。  
  
　　优先级的算法：  
  
　　每个规则对应一个初始"四位数"：0、0、0、0  
  
　　若是 行内选择符，则加1、0、0、0  
  
　　若是 ID选择符，则加0、1、0、0  
  
　　若是 类选择符/属性选择符/伪类选择符，则分别加0、0、1、0  
  
　　若是 元素选择符/伪元素选择符，则分别加0、0、0、1  
  
　　算法：将每条规则中，选择符对应的数相加后得到的”四位数“，从左到右进行比较，大的优先级越高。　　

### 需注意的：

　　①、!important的优先级是最高的，但出现冲突时则需比较”四位数“;  
  
　　②、优先级相同时，则采用就近原则，选择最后出现的样式;  
  
　　③、继承得来的属性，其优先级最低;  
  
　　!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性  
  
　　*css选择器使用强烈建议采用低权重原则，利于充分发挥css的继承性，复用性，模块化、组件化。

## css选择器的解析原则

         以前一直认为选择器的定位DOM元素是从左向右的方向，查看了网上的相关资料之后才发现原来自己一直都是错的。郑重的声明选择器定位DOM元素是从右往左的方向，这样的好处是尽早的过滤掉一些无关的样式规则和元素 。[为什么CSS选择器是从右往左解析 ？？？](http://blog.csdn.net/jinboker/article/details/52126021 "http://blog.csdn.net/jinboker/article/details/52126021")  
  

## 简洁、高效的css

        所谓高效就是让浏览器查找更少的元素标签来确定匹配的style元素。

      1.不要再ID选择器前使用标签名

        解释：ID选择是唯一的，加上标签名相当于画蛇添足了，没必要。

      2.不要在类选择器前使用标签名

      解释：如果没有相同的名字出现就是没必要，但是如果存在多个相同名字的类选择器则有必要添加标签名防止混淆如（p.colclass{color：red;} 和 span.colclass{color:red;}

      3.尽量少使用层级关系；

         #divclass p.colclass{color:red;}改为  .colclass{color:red;}

      4.使用类选择器代替层级关系（如上）


## link和@import的区别
1.link属于html标签。@import在css中使用表示导入外部样式表

2.页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载

3.import只在IE5以上才能识别，而link是HTML标签，无兼容问题

4.link方式的样式的权重 高于@import的权重

5.link 支持使用javascript改变样式 （document.styleSheets），后者不可

## 有哪些⽅式（CSS）可以隐藏⻚⾯元素

1.使用opacity:0

2.使用display:none

3.使用visibility: hidden

4.使用position; z-index: -999

5.使用transform: scale(0,0)

## em\px\rem区别

1.em是相对长度单位。
>1. em的值并不是固定的；
>2. em会继承父级元素的字体大小。

2.rem是CSS3新增的一个相对单位（root em，根em）
>区别在于使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素

3.px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。
> PX特点
>1. IE无法调整那些使用px作为单位的字体大小；
>2. 国外的大部分网站能够调整的原因在于其使用了em或rem作为字体单位；
>3. Firefox能够调整px和em，rem，但是96%以上的中国网民使用IE浏览器(或内核)。

## 块级元素⽔平居中的⽅法
1.利⽤定位+margin:auto

2.利用margin: 0 auto实现块级元素水平居中

3.利用text-align: center;实现块级元素内部的行内元素水平居中

4.利⽤定位+transform

5.flex布局

6.grid布局

7.table布局

## css有⼏种定位⽅式
1.static 静态定位

2.relative 相对定位

3.absolute 绝对定位

4.fixed 固定定位

5.sticky 粘性定位

## 如何理解z-index
z-index 属性是用来调整元素及子元素在 z 轴上的顺序，当元素发生覆盖的时候，哪个元素在上面，哪个元素在下面。通常来说，z-index 值较大的元素会覆盖较低的元素。
z-index 的默认值为 auto，可以设置正整数，也可以设置为负整数，如果不考虑 CSS3，只有定位元素(position:relative/absolute/fixed)的 z-index 才有作用，如果你的 z-index 作用于一个非定位元素(一些 CSS3 也会生效)，是不起任何作用的

## 如何理解层叠上下文
层叠上下文，你可以理解为 JS 中的作用域，一个页面中往往不仅仅只有一个层叠上下文(因为有很多种方式可以生成层叠上下文，只是你没有意识到而已)，在一个层叠上下文内，我们按照层叠水平的规则来堆叠元素。
>一共有三种大的类型创建层叠上下文：

>1.默认创建层叠上下文
>>只有 HTML 根元素，这里你可以理解为 body 标签。它属于根层叠上下文元素，不需要任何 CSS 属性来触发。

>2.需要配合 z-index 触发创建层叠上下文
>>position 值为 relative/absolute/fixed(部分浏览器)
>>flex 项(父元素 display 为 flex|inline-flex)，注意是子元素，不是父元素创建层叠上下文

>3.不需要配合 z-index 触发创建层叠上下文
>>由 CSS3 中新增的属性来触发的 
>>例如元素的透明度 opacity 小于1 ,
>>transform属性的值不是 none 等

## 清除浮动有哪些⽅法

1.额外标签法（在最后一个浮动标签后，新加一个标签，给其设置clear：both；）（不推荐）

2.父级添加overflow属性（父元素添加overflow:hidden）（不推荐）

3.使用after伪元素清除浮动（推荐使用）

4.使用before和after双伪元素清除浮动

## 你对css-sprites的理解
雪碧图也叫CSS精灵， 是⼀CSS图像合成技术，开发⼈员往往将⼩图标合并在⼀起之后的图⽚称作雪碧图。

好处：
减少加载多张图⽚的 HTTP 请求数（⼀张雪碧图只需要⼀个请求） 提前加载资源

不⾜：
CSS Sprite维护成本较⾼，如果⻚⾯背景有少许改动，⼀般就要改这张合并的图⽚ 加载速度优势在http2开启后荡然⽆存，HTTP2多路复⽤，多张图⽚也可以重复利⽤⼀个连接通道搞定

## 你对媒体查询的理解
响应式布局

CSS3 Media Query（媒体查询）@media可以根据不同的屏幕尺寸设置不同的样式，页面布局分别适应移动端、pc端等，在调整浏览器的大小，页面会根据媒体的宽度和高度来重新布置样式。媒体查询可以用于检测很多东西：自动检测viewpoint（视窗）的宽度和高度；设备的宽度和高度；旋转方向（智能手机横屏或竖屏）；分辨率大小。

## 你对盒模型的理解
盒模型的基本概念：CSS盒子模型也叫做框模型，具备内容(content)、填充(padding)、边框(border)、边界(margin)这些属性。在CSS中，每一个元素都被视为一个框，而每个框都有三个属性

盒子模型 = 内容区域(height,width)+内边距(padding)+边框(border)+外边距(margin)同时一个盒子大小的计算方式也是如此。

## 标准盒模型和怪异盒模型有什么区别
标准盒与怪异盒的区别在于他们的总宽度的计算公式不一样。

标准模式下总宽度=width+margin（左右）+padding（左右）border（左右）

怪异模式下总宽度=width+margin（左右）（就是说width已经包含了padding和border值）

当设置为box-sizing:content-box时，将采用标准模式解析计算，也是默认模式；当设置为box-sizing:border-box时，将采用怪异模式解析计算

## 谈谈对BFC(Block Formatting Context)的理解
即块级格式化上下文，它是页面中的一块渲染区域，并且有一套属于自己的渲染规则：
- 内部的盒子会在垂直方向上一个接一个的放置

- 对于同一个BFC的俩个相邻的盒子的margin会发生重叠，与方向无关。

- 每个元素的左外边距与包含块的左边界相接触（从左到右），即使浮动元素也是如此

- BFC的区域不会与float的元素区域重叠

- 计算BFC的高度时，浮动子元素也参与计算

- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然

## 为什么有时候⼈们⽤translate来改变位置⽽不是定位
translate是transform的一个值。改变transform或者opacity不会触发浏览器重新布局，或者重绘，只会触发复合。

绝对定位会触发回流，进而触发重绘，所以说在使用绝对定位时会触发重绘和回流操作。

transform使浏览器为元素创建一个GPU图层，但是改变绝对定位会使用到CPU，因此translate更加高效，可以缩短平滑动画的绘制时间。

translate改变元素时，元素依然会占据原始位置，但是绝对定位不会发生这样的情况。

## 伪类和伪元素的区别是什么

伪类:
- 选择器前面带":"前缀
- 用于已有元素处于某种状态时（滑动、点击等）为其添加对应的样式

伪元素：
- 选择器有“ :: ”为前缀。
- 用于创建一些不在DOM树中的元素，并为其添加样式
## 你对flex的理解
flex 布局是 flexible box 的缩写 ，意思为 弹性布局 用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为弹性布局。

容器默认存在两条轴线，即 主轴（main axis） 和 交叉轴（cross axis） 。

轴的轴线方向是由 flex-direction 决定的，可简单的理解为 x（row） 和 y（column） 两个方向，交叉轴是垂直于主轴方向的轴线。

flex 弹性盒模型的优势在于开发⼈员只需要声明布局应该具有的⾏为，⽽不需要给出具体的实现⽅式，浏览器负责完成实际布局，当布局涉及到不定宽度，分布对⻬的场景时，就要优先考虑弹性盒布局。
## 关于CSS的动画与过渡问题
过渡需要触发一个事件才会随着时间改变其CSS属性

动画在不需要触发任何事件的情况下，也可以随时间变化来改变元素CSS属性

过渡只有一组（两个：开始-结束） 关键帧，动画可以设置多个