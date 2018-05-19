---
layout: post
title: 深入理解css之line-height
date: 2018-05-19
categories: css, 前端
tags: css
---

* content
{:toc}

### 前言

行高，顾名思义是一行文字的高度，而从规范上来说则是两行文字基线之间的距离。行高是作用在每一个行框盒子(line-box)上的，而行框盒子则是由内联盒子组成，因此，行高与内联元素可以说是非常紧密，行高直接决定了内联元素的高度（注意：这里的内联元素不包括替换元素）；对于块级元素和替换元素，行高是无法决定最终高度的，只能决定行框盒子的最小高度。

### x、x-height以及ex

字母x在css里面扮演着一个很重要的角色，因为字母x的下边缘就是基线所在的位置。而x-height指的就是字母x的高度，ex是一个尺寸单位，其大小是相对字母x的来计算的，即1ex就表示1个字母x的高度。如下图所示：

![x-height](/images/posts/css/line-height/x-height.png){:width="300px"}

我们在平时的开发中很少用到ex，因为ex是个相对单位。对于相对的东西，我们总是感觉很难控制，但这并不表明ex就一点用处都没有。我们可以利用ex就是一个x-height的特性来实现图标与文字的垂直居中，这样如论字体大小如何变化，都不会影响垂直居中的效果。代码如下：

```css

.icon-arrow {
    display: inline-block;
    width: 20px;
    height: 1ex;
    background: url(down.png) no-repeat center;
    background-size: contain;
}

```

```html
<div>
    <span>我是一段文本</span>
    <i class="icon-arrow"></i>
</div>
```

效果如下：

![ex](/images/posts/css/line-height/ex.png){:width="110px"}

### line-height的属性值

* normal
* 数字
* 长度
* 百分比

#### normal

normal为line-height的默认值，但并不是一个固定的值，而是会受font-family的影响，对于“微软雅黑”，其值为1.32；而对于“宋体”，其值为1.141。由于不同操作系统，不同浏览器所使用的字体不一样，所以最终line-height的具体值会不一样，因此这个属性作用不大。

#### 数字

我们可以设置line-height: 1.5。其意思是说line-height的最终大小为 1.5* font-size，一般建议使用该值来设置line-height。

#### 长度

长度用的最多的就是px与em，em跟数字一样，都是相对于font-size来计算的。

#### 百分比

百分比也是相对于font-size来计算的。


相信细心的小伙伴已经发现了，数字，em以及百分比都是相对于font-size来计算的，既然这样，为什么还要多此一举设置另外两个属性呢。原因就在于它们的继承方式是不一样的。对于数字，父元素设置了1.5，则子元素也是会继承1.5。但如果父元素设置的是1.5em，假设父元素font-size是20px，则父元素line-height是30px，同时子元素的line-height也是30px，也就是说子元素继承的是父元素计算后的line-height值。因此，这也是为什么上面推荐使用数字而不是em或百分比的原因了。

### 行距与半行距

很多开发人员开还原设计图的时候，往往没有考虑到行距的影响，因此开发出来的页面很多时候都与设计图不符合，总会差那么几个像素。那么什么是行距呢，我们可以想象一下在文字排版的时候，如果行与行之间的间距为0，则文字是会紧紧贴在一起的，因此，行距就是用来协助排版的。行距的计算为：line-height - em-box，em-box指的是1em的大小，因此行距可以表示为：line-height - font-size，假设line-height为1.5，font-size为20，则行距为：
1.5*20 - 20 = 10，则最终行距为10，而这个行距会平均作用于文字的上边和下边。但em-box我们是无法感知这个盒子在哪的，而内容区域我们则可以理解为我们选中文字后的背景色所在区域，而当字体是宋体的时候，内容区域和em-box是等高的，因此我们可以利用此揪出ex-box的藏身之处。如下图所示：

![ex](/images/posts/css/line-height/line-space.png){:width="350px"}

正是因为行距的存在，我们给元素设置margin值时，要减去相应的半行距值，这样才能比较精确地还原设计图。

### line-height的应用

大部分时候，我们设置line-height，都是为了垂直居中对齐。但这里的居中，只能说是近似居中，从上面的图可以看出：行距是上下均分的，但是内容区域不是，一般来说，文字都是偏下的。我们平时设置字体一般都是12-20像素，这么小的像素值，给出line-height值后，由于上下相差不大，因此感觉上是垂直居中的。line-height除了可以作为单行文本的居中对齐外，多行文本也是可以的，代码如下：

```css

.box {
    width: 400px;
    line-height: 400px;
    padding: 0 10px;
    border: 1px solid #ccc;

}
.text {
    display: inline-block;
    line-height: 1.3;
    font-size: 14px;
    vertical-align: middle;
}

```

```html 

<p class="box">
    <span class="text">这是一段很长很长的文字，这是一段很长很长的文字，这是一段很长很长的文字，这是一段很长很长的文字，这是一段很长很长的文字</span>
</p>

```

效果如下：

![ex](/images/posts/css/line-height/line-height.png){:width="420px"}

前面的文章有说过，每一个行框盒子前面都有一个看不见的，规范称之为“strut”的东西。我们给.box设置了line-height为400px，则这个“strut”的line-height也会继承为400px。然后我们给.text设置inline-block，这样我们就可以重置.box设置的line-height，又因为ineline-block保持了内联特性，因此我们可以设置vertical-align以及产生“strut”，从而实现近似垂直居中对齐。

### 总结

* 介绍了字母x在css中的地位以及ex的应用
* line-height各种不同的属性值以及数字、em和百分比的不同之处
* 行距在line-height的作用
* line-height实现单行垂直居中和多行垂直居中

