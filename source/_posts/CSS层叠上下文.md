---
title: CSS层叠上下文
date: 2017-08-22 00:19:29
tags: CSS层叠
categories: CSS
---

​	最近看到张鑫旭大神的关于css元素层叠显示的文章，感到收获了很多，对于css元素堆叠有了深入的理解，下面是我做的写笔记。

###  层叠上下文

普通元素使用css的特殊属性可以创建层叠上下文，~~创建层叠上下文的元素的层级会高于普通的元素~~ ，这就好像，当我们眼睛看电脑的时候，创建了层叠上下文的元素离我们的眼睛更近了。当层叠上下文与普通元素叠加在一起的时候，层叠上下文元素会叠放在普通元素的上面。

在上面的这段话中，自己的理解是不正确的，创建了层叠上下文的元素只是有了层次，但是，并不能说，层叠上下文元素就一定到天然高于普通的元素。关于比较，还是要看 层叠顺序来的。

###  层叠水平

层叠水平类似 'level' , 层叠水平的大小决定了同一层叠上下文元素下元素在 z 轴上的显示顺序，所有的元素都有层叠水平都有层叠水平，但是对于普通元素，讨论其层叠水平是没有意义的。

普通元素的层叠水平优先由层叠上下文来决定，层叠水平的比较仅在层叠上下文中比较才有意义。

### 层叠顺序

层叠顺序定义的是在相同的层叠上下文中  **元素发生层叠的时候** ，特定的显示顺序, 层叠顺序图表引入顺序如下:

![ ]( CSS层叠上下文/层叠规则.png ) 

> 注意: 上面有句话 “当元素发生层叠的时候‘ ，如果元素没有创建层叠上下文，没有发生层叠的现象，下面的层叠顺序是不起作用的，但是，使用 `dispplay: inline-block` 是个例外，我想是因为在 CSS 中 `内容主要，样式次之`的原因吧， `inline-block`  会被认为是内容 ， `block` 会被认为属于布局的样式

如下代码所示:

```
<div class="con">
    <div class="block2">
    </div>
    <div class="block1">
    </div>
</div>
<style>
   .block1 {
       width: 200px;
       height: 200px;
       background-color: blue;
   }
    .block2 {
        display: inline-block;
        margin-bottom: -100px;
        width: 200px;
        height: 200px;
        background-color: red;
    }
</style>
```

效果如下:

![''](CSS层叠上下文/Snipaste_2017-08-22_23-40-52.png)

如果正常情况下，因为在 DOM 文档中，block1 元素位于 block2 元素下面，因此block 1 应该遮挡住 block2 才是，但是为什么会出现这种情况呢？

因为我们给 block2元素添加了 `display: inline-block` 属性，根据上面的层叠规则图我们可以知道，应用到的 inline-block元素层叠水平上要高于 block 元素，因此 block2 元素会覆盖掉 block1 元素，这就是 使用层叠顺序的作用，用于比较同一层叠水平下的元素顺序。

> 下面是两条非常重要的层叠准则如下 :
>
> 1.谁大谁上，当两个元素位于同一层叠上下文中的时候，按照层叠顺序，层叠水平大的要覆盖掉层叠水平小的元素。（要注意这里面当元素位于同一个层叠上下文的时候，才会按照层叠顺序来创建）
>
> 2.后来居上，当两个元素的层叠水平一致的时候，并且层叠顺序一致的时候，在DOM流中 后面的元素会覆盖掉前面的元素

### 创建层叠上下文

如何创建层叠上下文呢？这里有三种途径：

1.页面的根元素( html )本身带有层叠上下文

对于页面的根元素 ` html` 本身具有层叠上下文的

2.对于包含有 `position: absolute`  `position: relative ` 的定位元素，当 z-index 不是auto 的时候，会创建层叠上下文。(注意这里，z-index 不能是 auto, 否则不能创建层叠上下文)

例如代码如下:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div class="con">
    <div class="block1"></div>
    <div class="block2"></div>
</div>
<style>
   .block1 {
       width: 200px;
       height: 200px;
       background-color: blue;
   }
   .block2 {
        margin-top: -100px;
        width: 200px;
        height: 200px;
        background-color: red;
   }
   // 第一种情况
   .block1 {
      z-index: 2;
   } 
   .blcok2 {
      z-index: 1;
   }
   // 第二种情况
   .block1 {
      position: relative;
      z-index: 0;
   } 
   .blcok2 {
      z-index: 1000;
   }
</style>
</body>
</html>
```

我们通过对于css样式的改变出现下面的这几种情况：

第一种情况:

这种情况下，实际上 z-index 没有起作用，因为这里面的两个元素都是普通元素，没有创建层叠上下文的元素因此 这里使用 `z-index` 进行比较没有起作用   **使用 `z-index`只对于层叠上下文的元素起作用**

![](CSS层叠上下文/层叠（1）_2017-08-24_00-08-06.png)



第二种情况：

使用 `position: relative`  和 `z-index` 创建了层叠上下文， ~~在层次上，层叠上下文的要高于普通元素~~，因为使用 `z-index: 0` 的层叠水平会高于 display:block  元素，因此会出现下面的效果

![](CSS层叠上下文/层叠（2）_2017-08-24_00-10-54.png)



3.使用其他特殊的 css3  属性创建的层叠上下文。

使用一些特殊的层叠CSS3属性也会创建层叠上下文：有这么几个:

1.当一个元素的 **父元素** 声明 `display: flex | display: inline-flex` 的时候，并且该元素的 `z-index` 不是 `auto` 而是数数值的时候，该元素变为层叠上下文元素；

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div class="con">
   <div class="block1">

   </div>
    <div class="block2">

    </div>
</div>
<style>
    .con {
        display: flex;
    }
    .block1 {
        width: 200px;
        height: 200px;
        background-color: blue;
    }
    .block2 {
        margin-left: -100px;
        width: 200px;
        height: 200px;
        background-color: red;
    }
     .block2 {
       }
     .block1 {
         z-index: 1;
    }
</style>
</body>
</html>
```

正常情况下，block2 元素 应该在block元素上面，因为dom流中 block元素位于 后面

但是，当我们给 block1 添加了一个 `z-index`  不为 auto的值的时候，情况发生变化了, Magic !

如下：

![](CSS层叠上下文/flex-z-index%20%20magic%20%202017-08-24_22-33-25.png)

这是因为添加了 `z-index` 元素就变成了层叠上下文了，在层叠顺序表中， z-index 大于 0 的层叠上下文在 层次上要高于 `display: block` 的普通元素，如果 `z-index` 为 -1 的时候，其实情况又不一样了。因为 z-index 为负值的时候是会小于 `display: block` 元素的。

2. 使用 CSS 中的 `opacity` 创建的层叠上下文元素:

   我们有时候会出现想让 不透明元素内的元素显示正常，

   ```
   <div class='con'>
   	<div class= 'text'>我是一段文字</div>
   <div>
   <sytle>
   .con{
     width: 100px;
     height: 100px;
     background-color: blue;
     opacity: 0.5;
   }
   .text {
     width: 100px;
     height: 100px;
     background-color: red;
     position: relative;
     z-index: -1;
   }
   </style>
   ```

   在实际上，元素内的文字也会被透明度影响，这被认为是 使用 `opacity` 创建了层叠上下文元素

   因为，在没有使用 `opacity` 的时候， 没有创建层叠上下文， 这时候 .text 元素是创建了层叠上下文的，因为 `z-index` 小于零，在层叠规则上小于 `block` 元素，所以会被遮盖掉。当我们对于 `class='con'` 的元素没有设置 `opacity`的时候，效果如下：

   ![](CSS层叠上下文/opacityCENGDIE%202017-08-25_23-16-27.png)

   这样效果的原因是因为：使用了 `position: relative` 和 `z-index: -1` 形成的层叠上下文，在层叠规则上要低于 其他的`block` 元素，因此图片中的红色方块就被遮挡住了；

   然而，当我们对元素应用 `opacity` 当 `opacity` 是不为 1 的数字的时候，我们发现层叠顺序发生了改变：  

   ![](CSS层叠上下文/opacity%20Mag%202017-08-25_23-27-13.png)

   Magic!

   如上,

   因为我们对于 `class = 'con'` 设置了 `opacity: 0.7` 创建了层叠上下文，其子元素 `class ='text' ` 虽然也是创建了层叠上下文，但是根据层叠上下文的规则，如果父元素也是创建了层叠上下文，那么子元素创建的层叠上下文要受到父元素的层叠上下文的制约；

   >上面对于使用 `opacity` 创建的层叠上下文，来解释子元素会受到父元素 `opacity` 的影响，实际上，我们也可以认为属性 `opacity` 具有继承性，子元素会继承父元素的 `opacity` 属性

   对于使用  `opacity` 会影响到子元素的效果，如果我们想让子元素不受到父元素  `opacity` 的影响， 解决办法有下面的几种方法：

   >借用 `background: rgba()` 了来实现

   使用 这个属性可以避免 后代子元素使用 `opacity` 造成的干扰效果，

   ```
   background: rgba(R,G,B,A)
   // R: 代表红色取值
   // G: 代表绿色取值
   // B: 代表蓝色取值
   // A: 代表透明度
   // R G B 代表 三原色, 数值为 0 - 255 使用这三种颜色的组合可以实现任何其他的颜色
   ```

   因此，上面的问题，我们使用 `rgba` 来解决：

   `background: rgba(0,0,255,0.5)` 

   >还有一种方法，是让被 `opacity` 不作用在父元素上

   html: 

   ```
   <div class='con'>
   	<div class='text'>
   		<p>我是要进行显示的元素</p>
   	</div>
   	<div class='back'></div>
   </div>
   ```

   CSS:

   ```
   .blo {
           position: relative;
           width: 100px;
           height: 100px;
       }
   .back {
           position: absolute;
           top: 0;
           left: 0;
           width: 100%;
           height: 100%;
           background-color: blue;
           opacity: 0.2;
       }
   ```

   ​

    效果如下:

   ![](CSS层叠上下文/Snipaste_2017-08-26_00-24-17.png)

   #### 层叠上下文的特性：

   层叠上下文元素有如下特性：

   - 层叠上下文的层叠水平要比普通元素高；
   - 层叠上下文可以阻断元素的混合模式；
   - 层叠上下文可以嵌套，内部层叠上下文及其所有子元素均受制于外部的层叠上下文。
   - 每个层叠上下文和兄弟元素独立，也就是当进行层叠变化或渲染的时候，只需要考虑后代元素。
   - 每个层叠上下文是自成体系的，当元素发生层叠的时候，整个元素被认为是在父层叠上下文的层叠顺序中。 