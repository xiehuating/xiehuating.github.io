---
layout: post
title: "[翻译] A Complete Guide to Flexbox"
description: "本文是对CSS flexbox布局的全面指导手册。此完全手册解释了flexbox相关的所有事项，专注于所有父元素（flex容器）和子元素（flex元素）。也包括了flex的历史、示例、样式、以及浏览器支持。"
date: 2019-12-04
tags: [css, html]
comments: false
share: false
---



**This article is a Chinese translation version of [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) .**



<br/>

**如果翻译中有谬误之处，请不吝指正~**

<br/>



------

[TOC]



# A Complete Guide to Flexbox | Flexbox完全指导手册

原文文章发表于2013年4月8日；更新于2019年11月20日。

<br/>

本文是对CSS flexbox布局的全面指导手册。此完全手册解释了flexbox相关的所有事项，专注于所有父元素（flex容器）和子元素（flex元素）。也包括了flex的历史、示例、样式、以及浏览器支持。

<br/>

## Background | 背景

`Flexbox layout`（ 即`Flexible Box`）模块（自2017年10起的一个W3C候选推荐）目标是提供一种更加有效的方式对容器中的items进行空间布局、对齐、分布，甚至在他们的尺寸不确定或者动态的情况下（这是选用“flex”这个单词的原因）。

`Flex`布局背后的主要思想是使容器能够更改其`items`的宽度/高度（和顺序），以最好地填充可用空间（主要是适应各种显示设备和屏幕尺寸）。 `Flex`容器会扩展`items`尺寸以填充可用的可用空间，或缩短它们尺寸以防止溢出。

最重要的是，与常规布局（基于垂直的`block`和基于水平的`inline`）相比，flexbox布局与方向无关。尽管这些样式对于页面效果很好，但是它们缺乏灵活性（没有双关语）来支持大型或复杂的应用程序（尤其是在方向更改，调整大小，拉伸，缩小等方面）。

**Note：**`Flexbox layout`最适合应用程序的组件和小规模布局，而`Grid layout`则用于更大规模的布局。

<br/>

## Basics & Terminology | 背景&术语

由于`Flexbox`是一个完整的模块，而不是单个属性，因此它涉及很多事情，包括其整个属性集。它们中的一些应放置在容器上（父元素，称为`Flex container`），而其他一些则应放置在子元素上（称为`Felx items`）。 

如果regular布局是基于`block`和`inline`流动方向，则`flex`布局是基于`flex-flow`方向。请查看规范中的该图，解释`flex`布局背后的主要思想。

![](/images/a-complete-guide-to-Flexbox/00-basic-terminology.svg)

`Items`将沿`main axis`（从`main-start`到`main-end`）或`cross axis`（从`cross-start`到`cross-end`）布置。

- `Main axis`- `Flex container`的`main axis`是伸缩项目沿其布置的主轴。当心，它不一定是水平的。它取决于`flex-direction`属性（请参见下文）。
- `main-start` | `main-end` - `flex items`被放置到容器中，以从main-start`到`main-end`的顺序摆放。
- `Main size` - `flex items`的宽度或高度，哪个在`main dimension`上，哪个就是`items`的`main size`。`flex items`的`main size`属性是“宽度”还是“高度”属性，以哪个尺寸在`main dimension`上为准。
- `Cross axis` - 垂直于`main axis`的轴称为`cross axis`。其方向取决于`main axis`方向。
- `cross-start`|`cross-end` - 被 `items`填满的`Flex lines`从`flex container`的`cross-start`侧开始，朝`cross-end`侧放置到容器中。
- `Cross-size` - `flex items`的宽度或高度，哪个在`cross dimension`上，哪个就是`items`的`cross size`。`cross size`属性是“宽度”还是“高度”，以哪个尺寸在`cross dimension`上为准。

<br/>

## Properties for the Parent（flex container）| 父元素属性

![](/images/a-complete-guide-to-Flexbox/01-container.svg)

### display

这定义了一个`flex container`；`inline`或`block`取决于给定的值。它为其所有直接子元素启用`flex`上下文。

```css
.container {  
	display: flex; /*or inline-flex*/ 
}
```

请注意，CSS列对flex容器无效。

### flex-direction

![](/images/a-complete-guide-to-Flexbox/flex-direction.svg)

这样就建立了`main-axis`，从而确定了将`flex items`放置在`flex container`中的方向。除了可选包装外，`Flexbox`是单向布局概念。可以将`flex items`想像为主要以水平行或垂直列布置。

```css
.container {  
  flex-direction: row | row-reverse | column | column-reverse; 
}
```

- `row`（default）：`ltr`下从左往右；`rtl`下从右往左
- `row-reverse`：`ltr`下从右往左；`rtl`下从左往右
- `column`：和`row`类似，从上往下
- `column-reverse`：和`row-reverse`类似，从下往上

### flex-wrap

![](/images/a-complete-guide-to-Flexbox/flex-wrap.svg)

默认情况下，所有`flex items`都将尝试放入一行中。您可以对此进行更改，并允许该属性根据需要包裹`items`。

```css
.container{ 
	flex-wrap: nowrap | wrap | wrap-reverse; 
}
```

- `nowrap`（default）：所有`flex items`将会在一行中。
- `wrap`：flex items将会被包裹在多行中，从顶部到底部。
- `wrap-reverse`：flex items将会包裹在多行中，从底部到顶部。

这李有一些flex-wrap的[demo](https://css-tricks.com/almanac/properties/f/flex-wrap/)。

### flex-flow（Applies to：parent flex container element）

这是`flex-direction`和`flex-wrap`属性的简写，它们共同定义了`flex container`的`main axis`和`cross axis`。默认值为`row nowrap`。

```css
.container{ 
  flex-flow: <‘flex-direction’> || <‘flex-wrap’>;
 }
```



### justify-content

![](/images/a-complete-guide-to-Flexbox/justify-content.svg)

这定义了沿主轴的对齐方式。 当一行上的所有`flex items`都可伸缩或可伸缩但已达到最大大小时，它可以帮助分配额外的剩余可用空间。 当项目溢出行时，它还对项目的对齐方式施加一些控制。

```css
.container {  
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly | start | end | left | right ... + safe | unsafe; 
}
```

- `flex-start` （默认值）: 从flex-direction`的开始方向打包`items`。
- `flex-end`: 从flex-direction`的结束方向打包`items`。
- `start`: 从 `writing-mode` 的开始方向打包`items`。
- `end`: 从 `writing-mode` 的结束方向打包`items`。
- `left`: 从容器左边方向打包`items`，除非在`flex-direction`上没有意义，那么它的行为就和`start`一样。
- `right`: 从容器右边方向打包`items`，除非在`flex-direction`上没有意义，那么它的行为就和`end`一样。
- `center`: 沿线居中`items`。
- `space-between`: `items`在线上平均分配； 第一项位于开始，最后一项位于结束。
- `space-around`: `items`在行中均匀分布，周围具有相等的空间。 请注意，从视觉上看，空间是不相等的，因为所有项目的两侧都具有相等的空间。 第一项相对于容器边缘有一个单位的空间，但是下一项之间有两个单位的空间，因为下一项的两侧具有自己的间距。
- `space-evenly`: `items`被分布，以使任意两个项之间的间隔（以及到边缘的间隔）相等。

请注意，浏览器对这些值的支持是细微差别的。 例如， `space-between`在某些版本的Edge从未获得支持，而Chrome还没有支持`start`、`end`、`left`、`right`。 MDN有[详细的图表](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)。 最安全的值是`flex-start`，`flex-end`和`center`。

您还可以将其他两个关键字与这些值配对：`safe`和`unsafe`。 使用`safe`机制可确保无论您执行哪种类型的定位，都不能推送元素以使其呈现屏幕外（例如，超出顶部），这种模式下内容也无法被滚动（称为“数据丢失” ）。

### align-items

![](/images/a-complete-guide-to-Flexbox/align-items.svg)

这定义了`flex items`如何沿当前行的`cross axis`布局的默认行为。 将其视为`cross axis`（垂直于`main-axis`）的`justify-content`版本。

```css
.container {  
  align-items: stretch | flex-start | flex-end | center | baseline | first baseline | last baseline | start | end | self-start | self-end + ... safe | unsafe; 
}
```

- `stretch` （默认值）: 拉伸以填充容器（仍然遵守最小宽度/最大宽度）。
- `flex-start` / `start` / `self-start`: items被放置在`cross axis`的起点。 它们之间的区别是微妙的，并且涉及关于`flex-direction`规则或`writing-mode`规则。
- `flex-end` / `end` / `self-end`: items被放置在`cross axis`的终点。 它们之间的区别是微妙的，并且涉及关于`flex-direction`规则或`writing-mode`规则。
- `center`: `items`在`cross-axis`上居中。
- `baseline`: `items`对齐，例如以它们的基线对齐。

`safe`和`unsafa`的修饰语关键字可以与所有其他关键字（注意[浏览器支持](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items)）结合使用，并可以帮助您防止元素对齐导致无法访问内容。

### align-content

![](/images/a-complete-guide-to-Flexbox/align-content.svg)

当`cross-axis`上有多余的空间时，决定如何对齐`flex contain`的行，类似于`justify-content`在`main-axis`内对齐单个项目的方式。

**注意**：只有一行`flex items`时，此属性无效。

```css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | start | end | baseline | first baseline | last baseline + ... safe | unsafe;
}
```

- `flex-start` / `start`: `items`被打包到容器开头处。 `flex-start`（更多支持）遵循`flex-direction`，而`start`遵循`writing-mode`方向。
- `flex-end` / `end`:  `items`被打包到容器结束处。 `flex-end`（更多支持）遵循`flex-direction`，而`end`遵循`writing-mode`方向。
- `center`: `items`在容器中居中。
- `space-between`: `items均匀分布； 第一行在容器的开头，而最后一行在容器的结尾
- `space-around`: `items`均匀分布，且每行周围有相等的间隔。
- `space-evenly`: `items`均匀分布， 每行间隔相等。
- `stretch` (default): 行拉伸以占用剩余空间。

`safe`和`unsafa`的修饰语关键字可以与所有其他关键字（注意[浏览器支持](https://developer.mozilla.org/en-US/docs/Web/CSS/align-content)）结合使用，并可以帮助您防止元素对齐导致无法访问内容。

<br/>

## Properties for the Children（flex items）| 子元素属性

![](/images/a-complete-guide-to-Flexbox/02-items.svg)

### order

![](/images/a-complete-guide-to-Flexbox/order.svg)

默认情况下，`flex items`按源顺序排列。 但是，`order`属性控制它们在`flex`容器中出现的顺序。

```css
.item {  
	order: <integer>; */\* default is 0 \*/* 
}
```

### flex-grow

![](/images/a-complete-guide-to-Flexbox/flex-grow.svg)

这定义了`flex items`在必要时增长的能力。 它接受一个无单位值作为一个比例。 它决定了`items`应在`flex`容器内部占用多少可用空间。

如果所有`items`的`flex-grow`都设置为1，则容器中的剩余空间将平均分配给所有子项。 如果其中一个孩子的值为2，则剩余空间将占其他子项的两倍（或者至少会尝试）。

```css
.item {
  flex-grow: <number>; */\* default 0 \*/* 
}
```

负数无效。

### flex-shrink

这定义了`flex items`在必要时收缩的能力。

```css
.item {
  flex-shrink: <number>; */\* default 1 \*/* 
}
```

负数无效。

### flex-basis

这定义了剩余空间分配之前元素的默认大小。 它可以是长度（例如20％，5rem等）或关键字。 `auto`关键字的意思是“参考`width`或`height`属性”（由`main-size`关键字临时完成，直到弃用）。 `content`关键字的意思是“根据项目的内容调整大小”-此关键字尚未得到很好的支持，因此很难测试，也很难知道其弟兄的max-content，min-content和fit-content的行为。

```css
.item {
  flex-basis: <length> | auto; */\* default auto \*/* 
}
```

如果设置为0，则不考虑内容周围的多余空间。如果设置为auto，则多余的空间将根据其flex-grow值进行分配。 

看到这个[图形](https://www.w3.org/TR/css-flexbox-1/images/rel-vs-abs-flex.svg)。

### flex

这是`flex-grow`，`flex-shrink`和`flex-basis`组合的简写。 第二个和第三个参数（`flex-shrink`和`flex-basis`）是可选的。 默认值为`0 1 auto`。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ] 
}
```

**建议您使用此简写属性**，而不是设置单个属性。 这个简写可以智能地设置其他值。

### align-self

![](/images/a-complete-guide-to-Flexbox/align-self.svg)

允许覆盖单个`flex items`默认对齐方式（或由`align-items`指定的对齐方式）。

请参阅`align-items`的解释以了解可用值。

```css
.item {  
  align-self: auto | flex-start | flex-end | center | baseline | stretch; 
}
```

请注意，`float`、`clear`和`vertical-align`对`flex items`没有影响。

<br/>

## Examples | 示例



<br/>

## Prefixing Flexbox | 前缀



<br/>

## Related Properties | 相关属性



<br/>

## Other Resources | 其他资源



<br/>

## Bugs | Bugs



<br/>

## Browser Support | 浏览器支持



------

<br/>

<br/>

<br/>

Interpreted by [Xie Huating](https://github.com/xiehuating/), 2019-12-04

<br/>

<br/>

<br/>