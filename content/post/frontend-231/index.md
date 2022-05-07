---
title: '毎日のフロントエンド 231'
date: 2022-05-07T20:40:49+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `padding`和`margin`是相对于父元素还是子元素

- `padding`和`margin`被设置为**百分比**的时候，这个百分比是相对于它们的**包含块元素**的宽度

#### 包含块

> 大多数情况下，包含块就是这个元素最近的祖先块元素的内容区，但也不是总是这样

- 包含块有分为*根元素包含块*和*其他元素*的包含块

##### 根元素包含块

- 根元素 html 的包含块是一个矩形,叫做初始化包含块(initial containing block)
- `html`外面还有空间，这个包含 html 的块就被称为初始包含块(initial containing block)，它是作为元素*绝对定位*和*固定定位*的参照物
- 对于连续媒体设备（continuous media），初始包含块的大小等于视口 viewport 的大小，基点在画布的原点（视口左上角）
- 对于分页媒体（paged media），初始包含块是页面区域（page area）, 初始包含块的 direction 属性与根元素的相同

##### 其他元素的包含块

- 大多数情况下，包含块就是这个元素*最近的祖先块元素的内容区*
- 确定包含块的过程完全依赖于这个包含块的 `position` 属性，大致分为下列场景：
  1. 如果 position 属性是 static 或 relative 的话，包含块就是由它的最近的祖先块元素（比如说 inline-block, block 或 list-item 元素）或格式化上下文 BFC(比如说 a table container, flex container, grid container, or the block container itself)的*内容区的边缘*组成的
  2. 如果 position 属性是 absolute 的话，包含块就是由它的最近的 position 的值*不是* static （fixed, absolute, relative, or sticky）的祖先元素的内边距区的边缘组成的
  3. 如果 position 属性是 fixed 的话，包含块就是由 viewport (in the case of continuous media) or the page area (in the case of paged media) 组成的
  4. 如果 position 属性是 absolute 或 fixed，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：
     - transform or perspective value other than none
     - will-change value of transform or perspective
     - filter value other than none or a will-change value of filter (only works on Firefox).

##### 元素包含块的作用

> 元素的尺寸和位置经常受其包含块的影响。对于一个绝对定位的元素来说（他的 position 属性被设定为 absolute 或 fixed），如果它的 width, height, padding, margin, 和 offset 这些属性的值是一个比例值（如百分比等）的话，那这些值的计算值就是由它的包含块计算而来的。

- 如果某些属性被赋予一个百分值的话，它的计算值是由这个元素的包含块计算而来的。这些属性包括盒模型属性和偏移属性：
  1. `height`, `top`, `bottom` 这些属性由包含块的 `height` 属性的值来计算它的百分值。如果包含块的 `height` 值依赖于它的内容，且包含块的 `position` 属性的值被赋予 `relative` 或 `static` 的话，这些值的计算值为 0
  2. `width`, `left`, `right`, `padding`, `margin`, `text-indent`(2018-05-27 修改)这些属性由包含块的 `width` 属性的值来计算它的百分值

## Tips

### 获取 div 到浏览器窗口的高度

- `domYoffset = element.getBoundingClientRect().y;`

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [CSS 包含块](https://segmentfault.com/a/1190000015653589)

- [Element.getBoundingClientRect() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)
