---
title: translate3d'
date: 2017-11-05 20:39:20
tags: transform
categories: CSS
---

### transform 特性

一个使用 `transform` 实现的垂直居中的代码如下:



```html
<div class="fDiv">
  <div class="cDiv"></div>
</div>
<style >
  .fDiv {
    width: 300px;
    height: 300px;
    position: relative
    background-color: blue;
  }
  .cDiv {
    position: absolute;
    background-color: red;
    top: 50%;
    left: 50%;
    width: 50%;
    height: 50%;
    transform: translate3d(-50%, -50%, 0); // translate(-50%, -50%)
  }
</style>
```

`transform` 这个的 css 元素实际上是可以允许我们对于元素进行旋转, 移动, 缩放, 或者平移

常见的 `transform 特性如下`

![](translate3d/tranform%20特性.png)

其中 translate 规定的是元素在 x, y ,z 轴上的位移

#### translate

>(x, y, z)
>
>length / percentage  

其中 x, y z 的单位可以是长度或者是百分比, 当以百分比进行比较的时候, 百分比相对的是元素本身的高度或者宽度

在上面的完全居中代码中

```css
position: absolute;
background-color: red;
top: 50%;
left: 50%;
width: 50%;
height: 50%;
transform: translate3d(-50%, -50%, 0); // translate(-50%, -50%)
```

在添加 `transform`之前, 方块是这样被放置的:

因为这里是定位, `top` 以及 `left` 被放置的时候的宽度以及高度的百分比是按照父元素的宽度和高度进行计算的

![](translate3d/notransform.png)

添加了`transform` 之后

![](translate3d/tranform%20居中.png)

因为这里使用 translate 定义的距离 x , y , z 的距离是根据元素本身的宽度和高度被定义的, 而同时使用 `left: 50%` 的时候元素被紧靠在父元素的中间垂线上, 使用 `translate` 的时候向左移动了元素的一半距离, 使得这个元素在水平距离上是居中设置的。