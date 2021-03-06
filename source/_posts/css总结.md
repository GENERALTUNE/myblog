---
title: css总结
date: 2017-10-30 09:24:17
tags: css
---

未来的CSS太让人兴奋了：一方面，是全新的页面布局方式；另一方面，是酷炫的滤镜、颜色等视觉效果。这些CSS，受开发者追捧，被杂志和博客文章铺天盖地地介绍。 
　　如果说这些特性是CSS华丽的一面，那我们来看看它朴实的一面：很不起眼的东西，如选择器、单位、函数（方法）。我经常说这是繁琐的东西，但我意思是它们能干漂亮的活，这就是我要分享的。 
　　怎么说呢，让我们看看这些效果最好的朴实的CSS细节——这些细节远远没有那些酷炫的CSS效果那么引人注目。它们有些已经存在一段时间了，但值得我们更好地认识，而有些则刚刚面世。虽然不起眼，但是它们可以提高我们的工作效率——以谦虚的姿态。 
相对单位 
　　聪明又有前瞻头脑的开发者们已经使用相对单位了——如em或者百分比——所以，开发者们了解这个问题：往往因为元素的继承性而需要使用计算器作为辅助工具来计算大小。例如，现在普遍的做法是给页面的字体设置全局尺寸，然后用相对单位来定义页面中其它的元素。CSS大概会这样写： 
html { font-size: 10px; } p { font-size: 1.4em; }

　　这样写是没问题，直到有个子元素需要设置一个不同的字体大小，比如，在这样的标签当中： 
The cat sat on the mat.

　　如果你要设置span的字体大小为1.2em，你需要做什么？拿出计算器，算算1.2除以1.4是多少，结果如下： 
p span { font-size: 0.85714em; }

　　这个问题不局限于em。如果用百分比来创建响应式的流式布局网站，而百分比是与容器相关的，所以，如果要定义一个元素为它的容器的40%，它的高是75%，宽则需要设置为53.33333%。 
　　很明显，这很不方便。 
根相关的长度单位 
　　为了修复字体大小定义的问题，现在可以使用单位rem（root em）。rem同样是相对单位，但是它所对应的是固定的基本值，这个固定的基本值也就是文档的根元素的字体大小（在HTML文件中，就是html元素）。假设和上个例子一样，同样设定10px的字体大小为根元素的大小，那么CSS这样写就OK了： 
p { font-size: 1.4rem; } p span { font-size: 1.2rem; }

　　这两个CSS规则都是相对于根元素的字体大小，这样的代码更加优雅和简便，特别是在设置简单的数值如10px或者12px的时候。这样和使用px值很相似，不同点在于rem是可扩展的。 
　　在整篇文章介绍的特性中，rem特性相对来说是兼容性比较好，高级浏览器都能支持，包括IE9在内，除了Opera Mobile。 
窗口相关的长度单位 
　　觉得rem单位很酷吧，如果还有另外一组单位能解决百分比的问题，那就更酷了。它和rem的道理相似，不同点在于，它相对的不是文档的根元素，而是相对于设备窗口本身的大小。 
　　这两个单位就是vh和vw，即是相对于窗口大小的高和宽。每个单位在前面加上数字，代表的是多少个百分比。 
div { height: 50vh; }

　　在上面的例子，高度被设定为窗口高度的一半。1vh相当于一个百分比的窗口高度，所以50vh即是50%的窗口高度。 
　　如果窗口大小变了，那么这个值也随之改变。这相对百分比来说，好处是不需要担心父容器，不管它的父容器如何，10vw的元素会一直是10%的窗口大小。 
　　相应地，有vmin单位，相当于vh或者vw的最小值，最近还宣布有vmax单位会被加到规范文档里面（虽然在这篇文章发布的时候还没有）。 
　　现在支持这个特性的有IE9+、Chrome和Safari 6。 
运算式的值 
　　如果你在做响应式的流式布局网站，经常会遇到混合单位的问题——用百分比设置栅格，但是又用固定像素宽度设置margin。如： 
div { margin: 0 20px; width: 33%;}

　　如果布局只用到padding和border，你可以使用box-sizing来解决，但是对于margin就无能为力了。更好、更灵活的方法是使用calc()函数，设置不同单位之间的数学方程式，如： 
div { margin: 0 20px; width: calc(33% - 40px);}

　　它不仅可以用来计算宽，还可以用来计算长度——如果有必要，还可以在calc()里面再加calc()。 
　　这个特性IE9+和Firefox都支持，Firefox需要加上 -moz- 前缀（在版本16或17可能不用加前缀），Chrome和Safari也支持，但需要加上 -webkit- 前缀。然而，移动Webkit还不支持。 
加载字体库的部分字体 
　　优越的性能往往很重要，尤其是市场上各种各样的移动设备——导致连接速度的差异和不确定性——更加体现了这个重要性。其中一个加快页面加载速度的方法，就是减少外部文件个数，@font-face的一个新属性unicode-range就是为此而生。 
　　这个属性就是unicode-range（编码范围），代表的是编码字体的参数范围。在加载外部文件的时候，只有那些被使用的字体才会被加载，而不是整套字体库。下面的代码演示了如何从foo.ttf字体库中仅加载三个字体： 
@font-face {font-family: foo;src: url(‘foo.ttf’);unicode-range: U+31-33;}

　　这点对于使用字体图标的页面尤其有用。我测试过，使用unicode-range，加载字体文件的时间平均减少了0.85秒，也不是小数目了。当然，你可能不会这么想。 
　　这个属性，目前可以在IE9+、Webkit浏览器（如Chrome和Safari）中运行。 
新的伪类 
　　单位和值都应该好好利用，但是，让我更兴奋的是选择器和伪类。完善的选择器模式，即使只有少数浏览器支持，都让我兴奋不已。引用乔布斯的话：你要把栅栏的里面修得和外面一样漂亮，即使别人看不到里面——因为你自己知道。 
　　我第一次使用:nth-of-type()的时候，简直是一次突破，就像我冲出了思想的桎梏。好吧，我有些夸张了。但有些新的CSS伪类，确实值得狂热一番。 
否定伪类 
　　你大概不知道 :not() 伪类的好，除非你亲自实践一番。带有参数的 :not() 其实就是普通的选择器——不是复合选择器。一组元素加上选择器 :not()，表示满足这个参数的元素会被排除出去。听起来有些复杂吧？但是实际上非常简单。 
　　假设：要对项目列表的奇数行进行选择，但是最后一行除外。如果是以前，需要这样写： 
li { color: #00F; } li:nth-child(odd) { color: #F00; } li:last-child { color: #00F; }

　　现在，通过设定:last-child作为否定伪类的参数，就可以把最后一个元素排除，这样少了一行代码，从而更加的简洁和易维护。 
li { color: #00F; } li:nth-child(odd):not(:last-child) { color: #F00; }

　　否定伪类看起来并没有什么惊人之处，你可以不用它，但是它还是挺实用的。我曾经把它用在基于Webkit的项目当中，优势还是挺明显的。说实话，它是我最喜欢的伪类之一。 
　　是的，我有最喜欢的伪类。 
　　在本文提到的特性当中，否定伪类是兼容性最好的，它被IE9+和高级浏览器支持（不需要加浏览器产商前缀）。如果你熟悉jQuery，你可能习惯用它——版本1.0开始就有了，以及相似的not()方法。 
“适用于”伪类 
　　:matches() 伪类可以用普通的选择器、复合选择器、逗号隔开的列表或任何的选择器组合作为参数。太棒了！但是，它能做什么？ 
　　:matches() 伪类最强大的地方就是聚合多行选择器。例如，要选择父容器里面其中几个不同子容器里面的p元素，在这之前，代码或许会写成这样： 
.home header p,.home footer p,.home aside p {color: #F00;}

　　有了:matches()伪类，就可以把共同点提取出来，缩减代码量。该例子里面，选择器的共同点是以home为起点、以p为终点，所以可以用:matches()把中间的所有元素集合起来。是不是有些困惑？看看代码就明白了： 
.home :matches(header,footer,aside) p { color: #F00; }

　　这其实是CSS4的一部分（确切地说，是CSS选择器第四等级），这份规范文档还提到将会有类似的语法（以逗号隔开的复合选择器）应用于:not()伪类。兴奋ing！ 
　　目前，:matches()可以在Chrome和Safari浏览器中运行，但是要加上前缀-webkit-，Firefox也支持，但是要按照旧的写法:any()，同时要加上-moz-前缀。 
你爱上这些朴实的CSS细节了吗？ 
　　这篇文章讲到的特性，最赞的一点是它们解决了现实的问题，从琐碎而繁复的选择器到建立响应式网站的新挑战。实际上，我期待每一个特性被使用到最普通的项目当中。（web前端学习交流群：328058344 禁止闲聊，非喜勿进！）

　　新特性如滤镜可能很直观很华丽，但是我更愿意发现隐藏在深处的实用小技巧。 
　　在积极探索的过程中，每一个特性可以让你的职业生涯更顺利——想到这里，就不会觉得繁琐了ted below are all the plug

## CSS相关

1、左边定宽，右边自适应方案：float + margin，float + calc

/* 方案1 */ 
.left {
    width: 120px;
    float: left;
}
.right {
    margin-left: 120px;
}
/* 方案2 */ 
.left {
    width: 120px;
    float: left;
}
.right {
    width: calc(100% - 120px);
    float: left;
}
2、左右两边定宽，中间自适应：float，float + calc, 圣杯布局（设置BFC，margin负值法），flex

.wrap {
    width: 100%;
    height: 200px;
}
.wrap > div {
    height: 100%;
}
/* 方案1 */
.left {
    width: 120px;
    float: left;
}
.right {
    float: right;
    width: 120px;
}
.center {
    margin: 0 120px; 
}
/* 方案2 */
.left {
    width: 120px;
    float: left;
}
.right {
  float: right;
  width: 120px;
}
.center {
       width: calc(100% - 240px);
       margin-left: 120px;
}
/* 方案3 */
.wrap {
    display: flex;
}
.left {
    width: 120px;
}
.right {
    width: 120px;
}
.center {
    flex: 1;
}
3、左右居中

行内元素: text-align: center
定宽块状元素: 左右 margin 值为 auto
不定宽块状元素: table布局，position + transform
/* 方案1 */
.wrap {
    text-align: center
}
.center {
     display: inline;
         /* or */
         /* display: inline-block; */
}
/* 方案2 */
.center {
    width: 100px;
    margin: 0 auto;
}
/* 方案2 */
.wrap {
    position: relative;
}
.center {
position: absulote;
    left: 50%;
    transform: translateX(-50%);
}
4、上下垂直居中：

定高：margin，position + margin(负值)
    不定高：position + transform，flex，IFC + vertical-align:middle
    /* 定高方案1 */
.center {
    height: 100px;
    margin: 50px 0;   
    }
/* 定高方案2 */
.center {
    height: 100px;
    position: absolute;
    top: 50%;
    margin-top: -25px;
}
/* 不定高方案1 */
.center {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
/* 不定高方案2 */
.wrap {
     display: flex;
     align-items: center;
}
.center {
width: 100%;
}
/* 不定高方案3 */
/* 设置 inline-block 则会在外层产生 IFC，高度设为 100% 撑开 wrap 的高度 */
.wrap::before {
content: '';
height: 100%;
display: inline-block;
         vertical-align: middle;
}
.wrap {
    text-align: center;
}
.center {
display: inline-block;  
         vertical-align: middle;
}
5、盒模型：content（元素内容） + padding（内边距） + border（边框） + margin（外边距）

延伸： box-sizing

content-box：默认值，总宽度 = margin + border + padding + width
border-box：盒子宽度包含 padding 和 border，总宽度 = margin + width
inherit：从父元素继承 box-sizing 属性
6、BFC、IFC、GFC、FFC：FC（Formatting Contexts），格式化上下文

BFC：块级格式化上下文，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此(按照这个理念来想，只要脱离文档流，肯定就能产生 BFC)。产生 BFC 方式如下
float 的值不为 none。
overflow 的值不为 visible。
position 的值不为 relative 和 static。
display 的值为 table-cell, table-caption, inline-block中的任何一个。
用处？常见的多栏布局，结合块级别元素浮动，里面的元素则是在一个相对隔离的环境里运行。

IFC：内联格式化上下文，IFC 的 line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的 padding/margin 影响)。
IFC中的line box一般左右都贴紧整个 IFC，但是会因为 float 元素而扰乱。float 元素会位于 IFC 与 line box 之间，使得 line box 宽度缩短。 同个 ifc 下的多个 line box 高度会不同。 IFC 中时不可能有块级元素的，当插入块级元素时（如 p 中插入 div ）会产生两个匿名块与 div 分隔开，即产生两个 IFC ，每个 IFC 对外表现为块级元素，与 div 垂直排列。

用处？

水平居中：当一个块要在环境中水平居中时，设置其为 inline-block 则会在外层产生IFC，通过 text-align 则可以使其水平居中。
垂直居中：创建一个 IFC，用其中一个元素撑开父元素的高度，然后设置其 vertical-align: middle，其他行内元素则可以在此父元素下垂直居中


GFC：网格布局格式化上下文（display: grid）
FFC：自适应格式化上下文（display: flex）
CSS2.1中只有BFC和IFC, CSS3中才有GFC和FFC。
到底什么是BFC、IFC、GFC和FFC，次奥

What's FC？
一定不是KFC，FC的全称是：Formatting Contexts，是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。
BFC
BFC(Block Formatting Contexts)直译为"块级格式化上下文"。Block Formatting Contexts就是页面上的一个隔离的渲染区域，容器里面的子元素不会在布局上影响到外面的元素，反之也是如此。如何产生BFC？
float的值不为none。 
overflow的值不为visible。 
position的值不为relative和static。
display的值为table-cell, table-caption, inline-block中的任何一个。 
那BFC一般有什么用呢？比如常见的多栏布局，结合块级别元素浮动，里面的元素则是在一个相对隔离的环境里运行。
IFC
IFC(Inline Formatting Contexts)直译为"内联格式化上下文"，IFC的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的padding/margin影响)
IFC中的line box一般左右都贴紧整个IFC，但是会因为float元素而扰乱。float元素会位于IFC与与line box之间，使得line box宽度缩短。 同个ifc下的多个line box高度会不同。 IFC中时不可能有块级元素的，当插入块级元素时（如p中插入div）会产生两个匿名块与div分隔开，即产生两个IFC，每个IFC对外表现为块级元素，与div垂直排列。
那么IFC一般有什么用呢？
水平居中：当一个块要在环境中水平居中时，设置其为inline-block则会在外层产生IFC，通过text-align则可以使其水平居中。
垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他行内元素则可以在此父元素下垂直居中。
GFC
GFC(GridLayout Formatting Contexts)直译为"网格布局格式化上下文"，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。 
那么GFC有什么用呢，和table又有什么区别呢？首先同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。
FFC
FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container），可惜这个牛逼的属性只有谷歌和火狐支持，不过在移动端也足够了，至少safari和chrome还是OK的，毕竟这俩在移动端才是王道。
Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。
伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局。
醉饮山林，自是闲暇白云间。笑红尘，总是爱恨贪嗔痴。若问人间逍遥在，风生之谷，莲开其中。
详细参考:[css3中的BFC,IFC,GFC和FFC ](https://www.jianshu.com/p/e75f351e11f8)

