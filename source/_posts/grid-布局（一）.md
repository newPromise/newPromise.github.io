---
title: grid 布局（一）
date: 2018-08-18 23:45:07
tags: grid 布局
categories: CSS
---

`grid` 布局是一种方便的用于创建网格布局的强大工具， 使用`grid` 布局可以很方便的构建网页结构， 本篇文章主要介绍`grid` 布局的基础知识。

### 基本属性

使用 `grid` 布局实现一个九宫格:

```html
<div class="content">
  <div class="item1"></div>
  <div class="item2"></div>
  ...
  <div class="item9"></div>
</div>
```

```css
.content {
    display: grid;
    width: 300px;
    height: 300px;
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
}
```

实现的九宫格如下所示:

![](grid-布局（一）/grid(2).png)

#### display: grid

使用 `display: grid` 的目的是用于声明当前元素使用 `grid` 布局构建;

常用值:

* `grid`: 生成一个块状网格
* `inline-grid`: 生成一个内联网格
* `subgrid`: 表示当前的网格容器继承自父级元素的网格容器

#### grid-template-columns,  grid-template-rows

这两个属性用于在声明 `grid` 的当前元素内部划分网格内容。后面的数值表明网格内容的长度大小， 数值之间的空格表示划分网格的网格线。

`grid-template-columns` : 用于在网格元素划分列， 后面的值表示划分列的宽度， 比如上面的九宫格代码中， 表示将当前的元素划分为 3 列， 且三列的宽度均是 100px;

`grid-template-row`: 使用效果类似于 `grid-template-columns`, 是对于 `grid` 元素行的划分。比如上面的代码中表示将 `grid` 元素划分为三行， 并且三行的高度均为 `100px`;

常用值:

* `<track-name><track-size><track-name><track-size>...`

`track-size`: 表示网格内容的宽度，可取值:

* `percentage` 
* `数值`
* `auto`: 网格宽度的剩余空间
* `fr`: 表示等份网格容器中的可用空间

`track-name`: 表示网格之间网格线的名称

式例:

```css
.content {
  grid-template-columns: [line-start] 50px [line1-start] 20% [line2-start] 1fr [line-end];
}
```

效果如下:

![](grid-布局（一）/grid(3).png)

**当 auto 和 `fr` 同时存在的时候， 优先级： `auto` > `fr`, 这个时候 , 声明 `auto` 的那一列宽度为 0**

#### grid-template-areas

使用 `grid-template-areas` 用来定义网络模板；

常用值:

* `grid-area-name` : 由网格项的 `grid-area` 指定的网格区域名称
* `.` 表示一个空的网格单元
* `none` 表示不定义网格区域

例如下面实现的一个网页布局:

```html
<div class="content">
  <div class="header"></div>
  <div class="menu"></div>
  <div class="body"></div>
  <div class="footer"></div>
</div>
```

```css
.content {
  width: 400px;
  height: 300px;
  display: grid;
  grid-template-columns: repeat(1fr);
  grid-template-rows: repeat(1fr);
  // 这里的 h, m , b, f是和下面声明的 `grid-area` 是一一对应的
  grid-template-areas: 
    "h h h h"
    "m . b b"
    "f f f f";
}
.header {
  grid-area: h;
  background-color: lightcoral;
}
.menu {
  grid-area: m;
  background-color: lightblue
}
.body {
  grid-area: b;
  background-color: lightslategray
}
.footer {
  grid-area: f;
  background-color: lightseagreen;
}
```

效果如下:

![](grid-布局（一）/grid(4).png)