---
title: CSS块状格式化上下文(BFC)
date: 2017-09-17 09:37:10
tags: bfc
categories: CSS
---

### BFC 的概念

bfc 被称作: 块状格式化上下文，通过创建 bfc 可以创建出一块独立的渲染区域，在这个区域内，bfc 仅仅对于 `blcok-level` 块状水平的 `box` 起作用，bfc 上下文规定了内部的块状水平 box 如何起作用，并且创建了块状格式化上下文的容器，内部元素的布局方式不会影响到外部的区域，因此，我们可以使用 bfc 的概念实现对于浮动的清除。

### BFC 的创建

创建块状格式化上下文的方式有以下几种:

* `float` 的值不为 `none`
* `position` 的值不为 `static` 或者 `relative`
* `display` 的值为 `table-cell`  `table-caption` `inline-block` `flex` `inline-flex` 中的其中一个
* `overflow` 的值不为 `visible` (通过设置 `overflow: hidden`)

我的理解： 对于创建了块状格式化上下文的元素，在元素内部相当于定义了一个单独的区域，在这个区域内，内部子元素的布局不会影响到外部的元素

### BFC 的布局规则

>In a block formatting context:
>
>- boxes are laid out one after the other, vertically, beginning at the top of a containing block. The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block boxes in a block formatting context collapse.
>- each box's left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box's line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself may become narrower due to the floats).

在BFC 中: 

* 垂直方向上，内部的 box 元素从包含块的顶部开始一个接一个的布局，两个相邻块之间的垂直距离是有 `margin` 特性决定的，在相邻的块状元素的垂直 `margin` 上会出现外边距折叠的问题
* 每一个块状元素的左边缘会紧紧跟在包含块的左边缘，甚至当有浮动元素存在的情况下也是一样的道理，除非这个块状元素创建了一个新的 BFC

### BFC 的应用

#### 1. 使用 BFC 用来解决外边距折叠问题

****

外边距折叠是特定的相邻的外边距会形成一个单独的外边距，`collpase margin` 发生在下面的这些情况:

* 元素为空元素
* `padding` 或者 `border` 为 0
* `no clearance to seperate them` ? 什么意思

更多详细的的内容 :  https://www.w3.org/TR/css3-box/#compact-boxes

对于外边距坍塌，最终形成的外边距遵循的是 `M-N` 原则: 

如果两个元素的外边距是正值, 最后得到的坍塌之后的外边距是两个边距的最大值 `M`

如果两个元素的外边距是负值，最后得到的坍塌之后的外边距是两个边距中的最小值 `N`

如果外边距是 `none` , 得到的坍塌之后的外边距是 0



存在边距坍塌的几种情况

1. 相邻的块状元素之间出现外边距坍塌
2. 父元素和内部的子元素出现的外边距坍塌

****

使用块状格式化上下文解决外边距层叠:

css 代码如下 :

```
<div class="container">
	<div class="b1">
	</div>
	<div class="b2">
	</div>
</div>
<style>
.container {
  background: blue;
}
.b1, .b2 {
  width: 100px;
  height: 50px;
  margin: 10px;
  background: red;
}
</style>
```



![](BFC/margin%20collapse.png)

如上图所示, 父元素`container` 内部的子元素和父元素的边界坍塌，导致子元素与父元素的边界贴合，并且在子元素之间。两个 `div` `margin`  都设为 10, 最后出现折叠之后的 `margin` 也变成了 10 ，出现了元素折叠的现象

如何解决： 使用 `overflow: hidden` 创建 bfc

当我们给父元素创建 bfc 之后, 

```
.container {
  overflow: hidden
}
```

可以看到:

![](BFC/父元素%20bfc.png)

这时候因为父元素创建了 bfc ，因此子元素的 margin 不会出现坍塌现象，因为创建了 bf c 的元素不会和任何其它的元素出现边距折叠现象

但是, 我们看到，元素内部的两个字元素还是出现了边距坍塌现象，按照上边的思想，我们可以给单独的一个子元素创建bfc

```
<div class="container">
	<div class="b1">
	</div>
	<div class="b3">
		<div class="b2">
		</div>
	</div>
</div>

<style>
.b3 {
  overflow: hidden
}
</style>
```

上面的代码中, 我们给子元素 `b2` 上创建了一个父元素，给这个父元素创建bfc 

最终结果如下:

 ![](BFC/子元素%20bfc.png)  

最终我们可以看到，因为对于 `b3` 创建了 bfc ， 因此， 对于 bfc 内部的元素的布局是不会影响到外部的子元素的  

#### 2.使用 BFC 进行浮动的清除

使用 BFC 进行浮动清除的原理还是一样的，利用位于块状上下文中的元素是不会影响到外部元素的特性，防止子元素设置浮动之后，父元素高度为 0 的情况

```
<div class="container">
	<div class="b1">
	我是浮动元素
	</div>
	<div class="b2">
	我是浮动的元素
	</div>
</div>
<style>
.container {
  background: blue;
  overflow: hidden;
}
.b1, .b2 {
  float: left;
  width: 100px;
  height: 50px;
  margin: 10px;
  background: red;
}
</style>
```

结果如下所示:

![](BFC/bfc%20清除浮动.png)  

如上图所示，使用了 bfc 清除了由于使用 `float:left` 造成的浮动现象  浮动元素的父元素高度依然存在

#### 3. 使用 BFC 实现两栏自适应布局 

在存在浮动的情况下,前面我们说过，包含块内部的子元素的左边缘会紧紧贴在其父元素的左边缘，即使存在浮动元素的情况下也是一样的，例如：

```
<body>
   <div class="container">
       <div class="aside"></div>
       <div class="main"></div>
   </div>
</body>
<style type="text/css">
   .container  {
        width: 300px;
   }
   .aside {
    width: 20%;
    float: left;
    height: 100px;
    background-color: red;
   }
   .main {
    width: 80%;
    height: 200px;
    background-color: blue;
   }
</style>
```



图片如下:

![](BFC/noBFC%20自适应.png)

即使 `aside` 浮动，`main` 还是会贴在左边缘，如果我们想要实现两栏的自适应布局，如何实现？

根据上面的布局规则， 我们可以给 `main` 创建一个块状格式化上下文，这样，`main` 的布局就不会受到 `aside` 元素的影响

```
.main {
  width: 80%;
  height: 200px;
  background-color: blue;
  overflow: hidden;
}
```

最终，我们得到了自适应布局: 从而得到了一个两栏布局

![](BFC/BFC%20自适应.png)