---
title: flex box特性笔记
date: 2019-02-25 13:35:58
tags:
- css
- 前端
- flex
---

# flex-box 特性笔记

flex box对我来说也不是什么新奇的东西了，是大二下的时候学习的知识了。用着用着从显性知识渐渐的沉淀到了隐性知识，所以说对它的一些细节也不大记得清了，所以写下笔记整理一下。

> 如果能看懂英文文档的话，关于flex-box的讲解这边有一片更加完全的[文章](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

## 我知道的

flex是一种快速方便整理布局的方法，只要一个标签被设为flex，那它所有的子元素也都是flex。但是这个特性并不会继续往下延续。

flex的默认排序是行排序，即flex-direction中的row。flex-direction:row的排序方法，顾名思义，是一行行横着顺序下来的。其他的排序方法值得一提的是column和他们的逆序row-reverse，column-reverse。具体表现请参考MDN,在此不再赘述。

在日常中flex box中对其子元素的长度操作的属性有：flex-grow, flex-shrink，对其子元素的排版常用的属性有： flex-wrap，justify-content, align-items。



## flex-wrap

flex-wrap属性的默认值是no-wrap，即默认情况下flex内部元素排成一行。在这种情况下，如果flex-box子元素的总和超过了其父元素的width，那么其内部子元素的宽度将成为一个定值：这个定值刚好让每个子元素按原来的宽度之比把父元素宽度的平分(其实本质是flex-box的宽度不够的时候就会压缩子元素，而这个压缩的设置和flex-shrink有关)。

> 然后这样就不会出现overflow的情况了，这东西还把我坑了一下，解决方法请见下节

## flex:flex-grow && flex-shrink&&flex-basis

flex-grow（默认：0）：若flex-box内部存在空余空间，则会把多余空间按flex-grow的比例分配给flex-grow不为零的子元素（即宽度膨胀）。

flex-basis（默认：元素本身的宽度）: 若你将flex-basis设为0px，那么他在flexbox的眼里就是一个宽度为0的元素，这样来其余flex-grow不为0的元素就可以肆意抢占它的空间。

flex-shrink(默认：1): 之前提到过，当一个flex-box内部的宽度不够的时候，其子元素会被压缩，而这个压缩的相关设置就是有flex-shrink属性来控制的，如果你将一个子元素的flex-shrink属性设置为0,那么这个子元素将不会被压缩(但是相对的，其他保持默认flex-shrink属性的子元素将会被压缩的更加厉害)。



关于以上三种属性，官方推荐使用flex属性来同样配置，flex属性是这样的：

``` css
flex:[flex-grow] [flex-shrink][flex-basis]
```



## order属性

flex还有一个平时不怎么用的属性是order。通常来说，在flex-box内部，子元素会按他们在html中的顺序排列，但是设置子元素的order属性可以调换顺序，order的默认值是为0

## 小技巧

综上所述，其实flex-box可以实现的布局相当多，这里贴几个个人使用的小技巧：

### 利用flex-basis+flex-grow实现响应式等比元素分布

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="css,result" data-user="duskcloudxu" data-slug-hash="GeZOKQ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="flex-test">
  <span>See the Pen <a href="https://codepen.io/duskcloudxu/pen/GeZOKQ/">
  flex-test</a> by duskcloudxu (<a href="https://codepen.io/duskcloudxu">@duskcloudxu</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 利用flex-grow+flex-order+伪元素实现更加内聚的圣杯布局（这名字挺奇怪的

<p class="codepen" data-height="298" data-theme-id="0" data-default-tab="css,result" data-user="duskcloudxu" data-slug-hash="zbqbYG" style="height: 298px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="flex-preview2">
  <span>See the Pen <a href="https://codepen.io/duskcloudxu/pen/zbqbYG/">
  flex-preview2</a> by duskcloudxu (<a href="https://codepen.io/duskcloudxu">@duskcloudxu</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>