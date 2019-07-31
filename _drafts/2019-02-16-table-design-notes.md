---
layout: post
title: "设计一个优秀的表格"
description: ""
date: 2019-07-31
tags: [ ux design, ui design ]
comments: false
share: false
---

# 设计一个优秀的表格

表格作为一种悠久历史的数据展示方式被应用在各个行业之中。在网页上，表格也是非常重要的组成部分。常见的网页是由文字、图标、图片、视频、表格、表单、图表等组成，在中后端管理系统中，表格更是占据了页面的绝大部分面积，是展示操作大量结构性数据的首选方式，普遍适用于各种业务场景，所以对于展示结构性数据而言除了表格外，你并没有其他选择。

在本文中，将会分享我对于表格的认识。梳理我之前做交互设计中涉及到的表格的问题，并且分享学习优秀表格设计文章中对我有帮助的知识点。表格作为一种古老的数据展示工具，理论和应用已经非常成熟，无论是在纸质媒介或电子设备、不同行业、或不同学科，所以在本文中基本没有原创的内容，更多的是知识的引用和分享，然后带上一些自己的看法。我参考的优秀文章在本文末尾列出。

另外，本文分享的内容偏向于表格数据的展示。更准确的说是表格数据如何在web浏览器中更好的展示，注意这边说的web浏览器更多指的是桌面端的web浏览器。对于移动端而言，因为屏幕宽度较窄，我并没有见到过在移动端单页面中很好的表格展示方式。

对于表格数据的编辑本文也不会提及，这是另外一个更大、更深奥的主题。一旦涉及到编辑，那么设计任务的复杂程度往往超乎想象、并且会特别不可控。桌面端的Office Excel、Numbers这类通用的表格编辑工具算得上是史诗级难度的设计任务，我没有做过，所以也没法分享什么有价值的内容。近几年web在线文档编辑工具发展迅速，微软Office online、谷歌文档、国内也有石墨，腾讯文档等中都有提供表格编辑工具；web端还有一些优秀的表格编辑框架，如[DateTables](https://datatables.net/)、[Handsontable](https://handsontable.com/)、[dhtmlxSpreadsheet](https://dhtmlx.com/docs/products/dhtmlxSpreadsheet/)等，这些都可以作为你的参考。

<br/>

## 1. 表格的特点

> “the patterns and exceptions should be obvious at a glance, at least once one knows what they are.” 
>
> “数据的规律性和例外情况应该一目了然，至少看一次后，人们就知道他们的含义”
>
> — — Ehrenberg, 1977, *Rudiments of Numeracy*

所以说一个好的表格对于阅读者来说应该是一目了然的，对于数据之间的关系是易于理解、不言自明的。



阅读表格的瞬间，会发生什么？

当阅读行为发生时，如果能进入人脑，你会发现复杂的处理过程在瞬间发生。所以在谈论设计之前，让我们先来看看阅读表格时，人的大脑都会处理哪些任务。

1. 阅读标题：用户在阅读表格时通常会简单略过行或列的表头。
2. 通读全篇：用户也许会先花时间扫描全篇，了解表格的整体结构，数据分类以及复杂性。
3. 视觉搜索：为了找到数据，用户会顺着一行或者一列直到发现交叉点的有用信息，当用户对表格的结构熟悉，或者上下文的表格结构相同的时候，视觉搜索会更迅速的完成。
4. 信息提取：找到目标数据后，用户就从表中提取了一条基本信息。
5. 理解：用户倾向用他们已有的知识去理解已从表格中获取的信息。
6. 确定类别和趋势：用户会从感知层面对相似的数据进行归类并寻找变化趋势。
7. 比较：用户将会比较数据，并发现规律。
8. 推断：为了更深层次的理解数据变化，用户往往会推断一些结论
9. 解释：用户将会从自身的知识库中提取信息，来解释数据的意义。
10. 回忆：用户会需要记住表格中的一些信息，以便在将来使用这些信息。
11. 决策：用户会以他们对数据含义的解释为基础，进行相关决策。



<br/>

## 2. 表格的组成

HTML中将表格的元素定义的非常清晰，网页中表格包含的所有元素都使用HTML标签表示出来了。

- 表格标题：`<caption>`
- 表头：`<thead>`
- 主体：`<tbody>`
- 脚注：`<tfoot>`
- 行：`<tr>`
- 列：`<col>` `<colgroup>`
- 单元格：`<td>` `<th>`



<br/>

## 3. 表格设计的原则

表格设计应该遵循以下原则：

- 在开始表格设计前
  - 理解表格设计的目的
  - 理解信息数据的含义
  - 搞明白表格最终会呈现给谁看？
- 遵循行业的规范
- 视觉上易于用户快速搜索数据、提取数据、理解数据、对比数据。
- 清晰的



<br/>

## 4. 设计优秀的表格

1. 满足阅读者的期望



**2. 依据设计目标，合理排序**



**3. 移除无用元素**



**4. 创造视觉层级**



**5. 大多数人只看整数**



**6. 为读者做好计算**



**7. 保证前后风格一致**



**8. 有条理的对齐**



**9. 采用适当对比度，区分数据和背景**



**10. 给列数减肥**



**11. 让数据更易于比较**



**12.将类似数据进行分组**



**13.有效的使用网格**



**14.强调最重要的数值**



**15.提供一个简短的描述**



**16.有效的使用空白空间**



**17/18.使用有意义的表头和标题。**



https://www.jotform.com/blog/data-tables-in-modern-web-design/

**Titles + Labels + Data = Data Table** 



**Highlight Important Columns and Rows** 



**Don’t Leave Blank Spaces** 





**Use Icons** 





**Stick to a Simple Grid** 





字体



对齐



图片



在表格中使用ICON



高亮重要的列和行



## 5. 大型复杂表格的设计

**Make Columns Movable** 



**Allow for Reordering of Columns** 



**Provide Search for Large Tables** 



**Provide Different Views** 



https://uxdesign.cc/design-better-data-tables-4ecc99d23356

**Fixed Header**



**Horizontal Scroll**





**Display Density**



Visual Table Summary



**Pagination**



**Hover Actions**



**Inline Editing**



**Expandable Rows**



**Quick View** / Modal



Multi-Modal



Row to Details



Sortable Columns



Basic Filtering



Filter Columns





Searchable Columns



Add Columns



Customizable Columns



<br/>

## 6. 表格设计Checklist

- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否
- [ ] 是否



<br/>

## 7. 值得探讨的问题

关于对齐

关于响应式

关于移动端



## 8. 小结

Data is becoming the raw material of the global economy. The pursuit of data drives the reinvention of antiquated industries. Energy, media, manufacturing, logistics, healthcare, retail, finance, and even government are undergoing a digital transformation.

However, data is meaningless without the ability to visualize and act upon it. The companies that survive the next decade will not only have superior data; they will have a superior user experience.

Good user interface design is based on human goals and behavior. The user interface in-turn effects behavior, which drives further design decisions. In subtle and unconscious ways, user experience alters how humans make decisions. What is seen, where it is presented, and how interactions are afforded, influence actions. It is important we make design decisions that lead to a better world, one data table design at a time.

<br/><br/><br/>

## 附录

### 参考文章

[**Effective design of data tables**](http://www.thinkui.co.uk/resources/effective-design-of-data-tables/)

[**Data Tables In Modern Web Design**](https://www.jotform.com/blog/data-tables-in-modern-web-design/)

[**How To Architect A Complex Web Table**](https://www.smashingmagazine.com/2019/02/complex-web-tables/)

[**Design better data tables**](https://uxdesign.cc/design-better-data-tables-4ecc99d23356)

[**Designing The Perfect Feature Comparison Table**](https://www.smashingmagazine.com/2017/08/designing-perfect-feature-comparison-table/)

[**Guidelines for Designing Tables**](http://understandinggraphics.com/design/data-table-design/) （[插图](https://www.behance.net/gallery/885004/Designing-Effective-Data-Tables)、[中文版](https://www.biaodianfu.com/guidelines-for-designing-tables.html)）

[**Data Tables Design Basics**](https://design-nation.icons8.com/intro-to-data-tables-design-349f55861803)

[**PRACTICAL TYPOGRAPHY**](https://practicaltypography.com/)



### 优秀Guideline中的表格

[**iOS HIG > Views > Tables**](https://developer.apple.com/design/human-interface-guidelines/)

[**macOS  HIG > Windows and Views > Table Views**](https://developer.apple.com/design/human-interface-guidelines/macos/windows-and-views/table-views/)

[**Material Guidelines > Components > Data tables**](https://material.io/design/components/data-tables.html#)

[**Atlassian Design > Product > Tables**](https://www.atlassian.design/guidelines/product/components/tables)

<br/>

### 表格组件库

[Bootstrap > Table](https://getbootstrap.com/docs/4.3/content/tables/)

[Element > Table](https://element.eleme.cn/#/zh-CN/component/table)

[Ant Design > Table ](https://ant.design/components/table-cn/)

[Semantic UI > Table](https://semantic-ui.com/collections/table.html)

[Material-UI > Table](https://material-ui.com/demos/tables/)

[DateTables](https://datatables.net/)

[Bootstrap Table](https://bootstrap-table.com/)

[embertable](https://opensource.addepar.com/ember-table/latest/docs/)

[Handsontable](https://handsontable.com/)

[dhtmlxSpreadsheet](https://dhtmlx.com/docs/products/dhtmlxSpreadsheet/)

<br/>

### 优秀表格设计案例

[Material Design > Device Metrics](https://material.io/tools/devices/)

[iPhone XS Tech Spects](https://www.apple.com/iphone-xs/specs/)

<br/>



