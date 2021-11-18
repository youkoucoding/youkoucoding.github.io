---
title: '毎日のフロントエンド　63'
date: 2021-11-18T10:37:36+09:00
description: frontend 每日一练
image: frontend-63-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十三日

## HTML

### **Question:** 什么是 html 的字符实体？版权符号代码怎么写

在 HTML 中，某些字符是预留的，这些预留字符必须被替换为字符实体.。 如： `&lt`; `&gt`;

版权符号： `&copy`;

## CSS

### **Question:** `position`的`absolute`和`fixed`共同与不同点分别是什么

1. `static`: 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）
2. `relative`: 生成相对定位的元素，通过`top`, `bottom`, `left`, `right` 的设置**相对于其正常（原先本身）位置进行定位**。可通过`z-index`进行层次分级。
3. `absolute`: 生成绝对定位的元素，**相对于 `static` 定位以外的第一个父元素**进行定位。元素的位置通过 `left`, `top`, `right` 以及 `bottom` 属性进行规定。可通过`z-index`进行层次分级。
4. `fixed`: 生成绝对定位的元素，相对于**浏览器窗口**进行定位。元素的位置通过 `left`, `top`, `right` 以及 `bottom` 属性进行规定。可通过 z-index 进行层次分级。

#### `relative`

定位为 `relative` 的元素脱离正常的文本流中，但**其在文本流中的位置依然存在**。
无论父级存在不存在，无论有没有 TRBL(`top right bottom left`)，均是以父级的左上角进行定位，但是父级的`Padding`属性会对其影响。

#### `absolute`

定位为`absolute`的层脱离正常文本流，但与`relative`的区别是其**在正常流中的位置不再存在**。

若想把一个定位属性为 absolute 的元素定位于其父级元素内，只有满足两个条件：

1. 设定 TRBL(`top right bottom left`)
2. 父级设定`position`属性

#### `relative`与`absolute`的主要区别

- `relative`定位的层总是相对于其最近的父元素，无论其父元素是何种定位方式
- `absolute` 总是相对于其最近的定义为`absolute`或`relative`的父层，这个父层并不一定是其直接父层。如果其父层中都未定义`absolute`或`relative`，则其将相对 body`进行定位

#### Summary

用`position`来布局页面，父级元素的`position`属性必须为`relative`，而定位于父级内部某个位置的元素，最好用 `absolute`。 因为它不受父级元素的`padding`的属性影响，当然也可以用 position，不过到时候计算的时候不能忘记 padding 的值。

> 绝对(absolute)定位对象在可视区域之外会导致滚动条出现。而放置相对(relative)定位对象在可视区域之外，滚动条不会出现。
> 使用`static` 定位或**无 position**定位的元素`z-index`属性是无效的

## JavaScript

### **Question:** 举例说明 javascript 的变量声明提升和函数声明提升

- 变量声明只提升声明 不提升赋值操作
- 函数声明 函数体整体被提升

## One more Question

### **Question:** css 伪类 与 伪元素

#### 伪类(`pseudo-classes`)

- 其核⼼就是⽤来选择 DOM 树之外的信息,不能够被普通选择器选择的⽂档之外的元素，⽤来添加⼀些选择器的特殊效果。 ⽐如`:hover` `:active` `:visited` `:link` `:visited` `:first-child` `:focus` `:lang` 等
- 由于状态的变化是⾮静态的，所以元素达到⼀个特定状态时，它可能得到⼀个伪类的样式；当状态改变时，它⼜会失去这个样式。 由此可以看出，它的功能和 class 有些类似，但它是基于⽂档之外的抽象，所以叫 伪类。

#### 伪元素(`Pseudo-elements`)

- DOM 树没有定义的虚拟元素
- 核⼼就是需要创建通常不存在于⽂档中的元素，
- ⽐如 `::before` `::after` 它选择的是元素指定内容，表示选择元素内容的之前内容或之后内容。
- 伪元素控制的内容和元素是没有差别的，但是它本身只是基于元素的抽象，并不存在于⽂档中，所以称为伪元素。⽤于将特殊的效果添加到某些选择器

#### 伪类与伪元素的区别

- 表示⽅法:

  1. CSS2 中伪类、伪元素都是以单冒号`:`表示
  2. `CSS2.1` 后规定伪类⽤`:`单冒号表示,伪元素⽤双冒号`::`表示
  3. `CSS3`，伪类与伪元素在语法上也有所区别，伪元素修改为以`::`开头。浏览器对以:开头的伪元素也继续⽀持，但建议规范书写为::开头

- 不同之处
  1. 伪类即假的类，可以添加类来达到效果
  2. 伪类其实就是基于普通 DOM 元素⽽产⽣的不同状态，是 DOM 元素的某⼀特征。
  3. 伪元素即假元素，需要通过添加元素才能达到效果
  4. 伪元素能够创建在 DOM 树中不存在的抽象对象，⽽且这些抽象对象是能够访问到的

> 是否需要添加元素才能达到效果，如果是则是伪元素，反之则是伪类。

- 相同之处
  1. 伪类和伪元素都**不出现**在源⽂件和 DOM 树中。也就是说在 html 源⽂件中是看不到伪类和伪元素的

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[HTML ISO-8859-1 参考手册](https://www.w3school.com.cn/charsets/ref_html_8859.asp)
