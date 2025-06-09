---
title: vertical-align
date: 2025-06-06 17:18:47
tags: vertical-align, line-height, CSS
categories: frontend, CSS, layout
---

### vertical-align

关于 `vertical-align` 的字面意思是垂直居中的意思， 其中 `vertical-align` 有下面几种支持的属性：

| 值               | 含义                                       |
| --------------- | ---------------------------------------- |
| `baseline`(初始值) | 一个元素的基线和父元素的基线对齐                         |
| `sub`           | 将元素作为一个下标，该元素的基线会相当于父元素的基线降低             |
| `super`         | 将元素作为一个上标， 该元素的基线会相当于父元素的基线升高            |
| `top`           | 把对齐的子元素的顶端与父元素顶端对齐。                      |
| `text-top`      | 类似于使用 `text-bottom` ， 将元素行内文本的顶端与父元素的顶端对齐 |
| `middle`        | 居中对齐，常用于图像的垂直居中                          |
| `bottom`        | 将元素行内框的低端与父元素的低端对齐                       |
| `text-bottom`   | 行内文本的底端与行框的低端对齐                          |
| `百分数`           | 将元素的基线相对于父元素的基线升高或者降低指定的量， 这里的百分数是指相对于该元素的 `line-height` 的百分数。 |
| `length`        | 使用 `length` 用于将元素升高或者降低指定的距离             |
| `inherit`       | 从父元素下继承属性                                |

*注意：这里的 `vertical-align` 影响的是行内元素， 行内块元素，以及表单元格的对齐，对于 块状元素不受 `vertical-align` 的影响。*

`vertical-align` 作用效果在图像垂直居中的时候的作用：

```html
<div class="wrap">
  <img class="img" alt="这是一张图片">
</div>
```

我们知道对于行内元素的垂直居中我们可以使用 `line-height = height` 的方法实现垂直居中的效果， 但是对于图像而言使用这种方法是失效的，例如下面的代码：

```css
.wrap {
    width: 200px;
    height: 300px;
    line-height: 300px;
    background-color: blue;
}
.img {
    width: 50px;
    height: 50px;
}
```

结果如下：

![](vertical-align/vertical-align.png)

如上图所示， 使用 `line-height = height` 并不能实现对于图片的垂直居中效果，为了实现图片的垂直居中， 在 `img` 元素上添加 `vertical-align:middle` 效果， 最终效果如下：

```css
.img {
    vertical-align: middle;
}
```

​                                                ![](vertical-align/vertical-align2.png)  

注意： 这里的 `vertical-align` 主要参照的是父元素的行高， 因此在设置 `vertical-align: middle` 的时候，需要将父元素的 `line-height` 设置为 父元素的 `height` 高度。

####  `vertical-align:middle`  

`vertical-align: middle` 经常用于图像的居中， 我们要注意的一点就是，当元素设置 `vertical-align: middle` 的时候，这个属性会将行内元素框的中点与父元素的基线上方 *0.5ex* 处的一个点进行对齐， 这里的 *1ex* 是相对于父元素的 `font-size` 进行定义的，例如下面这个例子：

```html
<span class="allDemo">
	<span class="demo1">我是一段文字</span><span class="demo2">我是第二段文字</span>
</span>
```

*对于 demo1 进行 `vertical-align: middle`*: 

```css
.allDemo {
	display: inline-block;
	background-color: lightslategray;
}
.demo2 {
	line-height: 100px;
	background-color: lightgreen;
}
.demo1{
	background-color: lightcoral;
	vertical-align: middle;
}
```

![](vertical-align/38161994-9af821b2-350b-11e8-898a-6074bbff3262.png)

如上图所示， `class = "demo2"` 这段文字， `vertical-align` 默认是 `baseline`,其元素框底端是与行框的基线对齐的，`demo1` 相比于 `demo2` 元素而言， 元素下移， 这是 `vertical-align: middle` 之后的结果， 图示如下:

![](vertical-align/38162080-78c48b88-350d-11e8-8d9c-378a8fc11606.png)

如果我们将父元素的 `font-size` 置为0， 我们将会看到下面的情况：

代码如下：

```css
.allDemo { font-size: 0px };
.demo1 { font-size: 16px };
.demo2 { font-size: 16px };
```



![](vertical-align/38162298-3cec1974-3511-11e8-8b4f-54709a46b049.png)

`vertical-align` 各属性作用位置如下：

![](vertical-align/38162270-cd4fcf8e-3510-11e8-9926-5cc120923396.png)



### line-height

###  `line-height` 与 `line box` 之间的关系
`line-height` 从字面意义上来讲， 是 `行高` 的意思，在页面上表现出来的就是一行文字的高度， 在介绍 `line-height` 之前，我们先来认识一下 `line boxes` 和 `inline boxes` 这两个东西。
#### `line boxes`  与 `inline boxes`  
`inline boxes` : 可以认为是包裹在 `inline` 元素外面的的一层外层， 例如 `span` 元素， `img` 图片元素等 `inline` 形式的元素，对于 `inline` 水平的元素，都会形成一层的 `inline boxes` 进行包裹。  
`line boxex` : 对于 `line boxes` 你可以认为 `line boxes` 用于包裹一行元素， 也就是说，对于一行 `inline` 水平的元素而言， 在外面有一个 `line boxes` 进行包裹， 如果一行有多个 `inline` 水平的元素，那么，这一行的 `line boxes` 就会包含有多个的 `inline boxex`。
**对于 `line boxes` 的元素的高度， 他的高度是获取该 `box` 下面的所有的 `inline boxes` 元素的高度 ， 比较获取他们中最大的高度， 最后这个最大的高度被认为是 `line boxes` 的最大高度**。

**这里 `inline boxes` 的高度是什么呢？ 就是今天我们要说的 `line-height`**.
行高具体来讲就是两行文字之间基线之间的距离：  
如下图所示：
![image](vertical-align/38088466-835a2b96-338e-11e8-82be-ac0a4a367222.png)
上面的图中红线就是表示所谓的基线， 关于我们另外一个css 的属性 `vertical-align` 改变的就是基线的高低大小。 

#### 使用 `line-height` 实现的垂直居中实现
我们经常使用 `line-height = height` 实现行内元素的垂直居中效果， 这里的 `height` ， 更为确切的说是  我们将要居中 `line boxes` 的高度进行居中， 因为对于行高而言具有一个垂直居中的性质。

#### 使用 `line-height = height` 在图片中垂直居中效果的失效。
html:
```html
<div class="demo">
        <img src="./picture.png" alt="垂直居中的图片" class="img">
</div>
```

CSS:
```css
.demo {
    display: inline-block;
    line-height: 150px;
    height: 150px;
    background-color: lightblue;
}
.img {	
    height: 50px;
}
```
最终结果如下：

![image](vertical-align/38089336-b99abea2-3391-11e8-81ee-5ffa5c6b736e.png)

并没有居中！

对于图片元素而言， 使用 `line-height = height` 并不能实现元素的垂直居中，要想实现这种效果， 就需要 `vertcial-align: middle` 出马了。

但是使用 `vertical-align:middle` 就能保证万无一失了吗？

### `font-size: 0px` 在图片居中时的应用

在讲解 `vertical-align:middle` 的时候， 我们使用 `vertical-align: middle` 实现了图片的垂直居中， 但是这个垂直居中只是近似的， 并不是真正的垂直居中。

```html
<div class="imgWrap">
	<img src="img.png" class="img">
</div>
```

```css
.imgWrap {
    line-height: 40px;
    background-color: lightblue;
}
.img {
    width: 50px;
    height: 30px;
    vertical-align: middle;
}
```

![](vertical-align/38165453-22d6400c-3546-11e8-9eb6-460cac7be56b.png)

我们看到，这个时候使用 `vertical-align:middle` 并没有实现真正的垂直居中， 原因是什么呢？

因为就如同我们刚才说的那样：

>当元素设置 `vertical-align: middle` 的时候，这个属性会将行内元素框的中点与父元素的基线上方 *0.5ex* 处的一个点进行对齐

我们想要的结果是将行内元素框的中点和父元素的中点进行对齐，而使用 `vertical-align` 的时候并不是这样， 为了解决这个问题， 我们使用了`font-size: 0`这个属性。

代码如下：

```CSS
.imgWrap {
  font-size: 0;
}
```

居中成功！

![mark](vertical-align/vertical-alignFontSize.png)

`font-size: 0px` 起的作用是：

1. 基线和中线之间的距离是根据字母  `x` 的高度进行计算的， 通过设置 `font-size` 为0， 使得 基线和中线在同一水平线上，从而使得图片元素框的中线和父元素的中线重合， 实现垂直对齐效果。
2. 使用 `vertical-align:middle` 对齐的不是父元素的基线， 而是基线上面  `0.5ex` 这样一个高度的点， 但是这个高度是由父元素的 `font-size` 来决定的， 这样就将这个高度置为 0 ， 从而使得图片中线和父元素基线对齐。   

### 参考链接

[张鑫旭：css行高line-height的一些深入理解及应用](http://www.zhangxinxu.com/wordpress/2009/11/css%E8%A1%8C%E9%AB%98line-height%E7%9A%84%E4%B8%80%E4%BA%9B%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%8F%8A%E5%BA%94%E7%94%A8/) 