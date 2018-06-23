---
layout: post
title: 深入理解css之vertical-align
date: 2018-06-23
categories: css, 前端
tags: css
---

* content
{:toc}

### 前言

vertical-align用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。也就是说，对于块级元素，vertical-align是不起作用的。

### vertical-align的各类属性值

vertical-align的属性值可以归为以下4类：

* 线类，如 baseline、top、middle、bottom；
* 文本类，如 text-top、text-bottom；
* 上标下标类，如 sub、super；
* 数值百分比类，如 10px、1em、5%；

#### 线类

baseline，baseline为vertical-align的默认值，其意思是指基线对齐，所谓基线，指的是字母x的下边缘，具体可看前文[深入理解css之line-height](https://segmentfault.com/a/1190000014936270)有讲解到，不懂的小伙伴建议先看看这篇文章。我们来看个例子，代码如下：

```css

.box {
    width: 100px;
    line-height: 100px;
    border: 1px solid #ccc;
}

```

```html

<div class="box">
    <span class="text">文本</span>
</div>

```

效果如下：

![baseline](/images/posts/css/vertical-align/baseline.png){:width="100px"}

由于baseline是默认值，所以可以不用写。.box的line-height为100px，这其实是给“strut”设置的（不懂strut概念的可以看看前面的文章[深入理解css盒子模型](https://segmentfault.com/a/1190000014692461)，简单点说就是每一个行框盒子都有一个看不见的节点，该节点继承了line-height），因此.text对齐于该节点的基线（可以想象成这个看不见的节点有一个字母x，而.text就是跟这个字母x的下边缘对齐）

关于baseline，有一个需要注意的地方就是inline-block元素，如果一个inline-block元素，里面没有内联元素，或者overflow不是visible，则该元素的基线是其margin底边缘；否则其基线就是元素里面最后一行内联元素的基线。例子如下：

```css
.text {
    display: inline-block;
    width: 100px;
    height: 100px;
    border: 1px solid #ccc;
}
```

```html
<div class="container">
    <span class="text">文本</span>
    <span class="text"></span>
</div>
```

效果如下：

![baseline2](/images/posts/css/vertical-align/baseline2.png){:width="200px"}


top，对于内联元素，指的是元素的顶部和当前行框盒子的顶部对齐；对于table-cell元素，指的是元素的顶padding边缘和表格行的顶部对齐。例子如下：

```css
.box {
    width: 100px;
    line-height: 100px;
    border: 1px solid #ccc;
}
.top {
    line-height: normal;
    vertical-align: top;
}
```

```html

<div class="box">
    <span class="top">文本</span>
</div>

```

效果如下：

![top](/images/posts/css/vertical-align/top.png){:width="100px"}

bottom，跟top类似，将顶部换成底部即可。

middle，这个属性值用得比较多。对于内联元素指的是元素的垂直中心点与行框盒子基线往上1/2x-height处对齐，简单点说就是字母x的中心位置对齐；对于table-cell元素，指的是单元格填充盒子相对于外面的表格行居中对齐。基本上所有字体中，字母x的位置都是偏下一点的，font-size越大偏移越明显，因此，字母x中心的位置不是行框盒子的中心，也就是说vertical-align只能实现近似垂直居中对齐。

#### 文本类

text-top，指的是盒子的顶部和父级内容区域的顶部对齐。

text-bottom，指的是盒子的底部和父级内容区域的底部对齐。

例子如下：

```css
.box {
    width: 300px;
    line-height: 100px;
    border: 1px solid #ccc;
    font-size: 20px;
}
.f12 {
    font-size: 12px;
}
.f16 {
    font-size: 16px;
}
.f20 {
    font-size: 20px;
}
.text-top {
    line-height: normal;
    vertical-align: text-top;
    width: 100px;
}
```

```html
<div class="box">
    <span class="f12">12px</span>
    <span class="f16">16px</span>
    <span class="f20">20px</span>
    <img class="text-top" src="./card.jpg"/>
</div>
```

效果如下：
![text-top](/images/posts/css/vertical-align/text-top.png){:width="300px"}

所谓内容区域，可以看成是鼠标选中文字后高亮的背景色区域，上面的例子中，由于父元素设置的是20px，所以图片的vertical-align设置text-top的时候，就可以看成是跟子元素为20px元素的内容区域顶部对齐。


#### 上标下标类

上标和下标对应着两个标签super和sub，super在上面，sub在下面，这两个属性值在数学公式和化学表达式中用得比较多，平时我们开发几乎用不到，也没啥好讲的。

#### 数值百分比类

vertical-align是支持数值的，并且兼容性也非常好，但大部分开发人员却不知道vertical-align支持数值。对于数值，正值表示由基线往上偏移，负值表示由基线往下偏移。而百分比则是基于line-height来计算的，百分比用得比较少，因为line-height一般都是开发人员给出的，这时候数值就可以精确定位元素，不需要再使用百分比再去计算一遍。使用数值的代码如下：

```css
.box {
    width: 300px;
    line-height: 100px;
    border: 1px solid #ccc;
    font-size: 20px;
}
.num {
    line-height: normal;
    vertical-align: 20px;
}

```

```html

<div class="box">
    <span class="num">文本</span>
</div>

```

效果如下：

![number](/images/posts/css/vertical-align/number.png){:width="100px"}


### vertical-align起作用的前提

vertical-align起作用是有前提条件的，这个前提条件就是：只能应用于内联元素以及display值为table-cell的元素。在css中，有些css属性是会改变元素的display值的，例如float和position: absolute，一旦设置了这两个属性之一，元素的display值就是变为block，因此，vertical-align也就失去了作用。下面这段代码这样写就是错的：

```css

   span {
        float: left;
        vertical-align: middle; /* 错误，该行代码无效 */
   }

```

另外，更多人遇到的是以下这种无效的情况：

```css

.box {
    height: 200px;
}
.box > img {
    height: 100px;
    vertical-align: middle;
}

```

```html 

<div class="box">
    <img  src="1.jpg" />
</div>

```

其实，不是vertical-align无效，而是前面所说的“strut”的影响，由于.box没有设置line-height，所以“strut”的line-height就非常小，比图片的高度小很多，vertical-align: middle没法发挥作用。这时给.box一个比较高的line-height，就会看到vertical-align起作用了：

```css

.box {
    height: 200px;
    line-height: 200px;
}

```

### vertical-align与line-height的关系

前面讲了，vertical-align的百分比值是根据line-height来计算的。但实质上，只要是内联元素，这两个元素都会同时在起作用。如下例子：

```css
.box {
    line-height: 32px;
}
.box > span {
    font-size: 24px;
}
```

```html

<div class="box">
    <span>文本</span>
</div>

```

从代码上看，好像.box的高度会是32px，但实质上.box的高度会比32px还要高。原因是"strut"继承了line-height: 32px，span也继承了line-height: 32px，但两者的font-size不一样，这就导致了"strut"的font-size比较小，而span的font-size比较大，也就是说它们的基线不在同一位置上，"strut"偏上一点，而span默认又是基线对齐，为此，span总体会往上移以便跟"strut"基线对齐，.box元素就是这样被撑高了。而解决方案可以有以下几种：

* span元素不使用基线对齐，可以改为top对齐
* span元素块状化
* line-height设置为0
* font-size设置为0


### 总结

* 讲解了vertical-align的各类属性值及其效果
* vertical-align起作用的前提是内联元素
* vertical-align与line-height都是同时作用在内联元素上的

