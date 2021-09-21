---
title: '毎日のフロントエンド　7'
date: 2021-09-21T10:52:54+09:00
description: frontend 每日一练
image: frontend-7-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七日

## HTML

**#Question:** iframe 框架都有哪些优缺点？

The `<iframe>` HTML element represents a nested browsing context, embedding another HTML page into the current one.

[`<iframe>`: The Inline Frame element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)

[iFrame — A love story. by Max Rafferty | Slices of Bread | Medium](https://medium.com/slices-of-bread/iframe-a-love-story-3dbe4e519b86)

Advantages of `<iframe>`:

- `iframe` can display the embedded web page _intact_.
- If there are multiple pages referencing iframe, you only need to modify the content of iframe, and then you can change the content of every page you call, which is convenient and fast.
- If the header and version of a web page are the same in order to unify the style, it can be written as a page, nested with iframe, which can increase code reusability.
- If you encounter slow loading third-party content such as icons and advertisements, these problems can be solved by iframe.
- 可以实现跨域，每个 iframe 的源都可以不相同（方便引入第三方内容）

Disadvantages of `<iframe>`:

- Many pages will be generated, which is not easy to manage.
- Iframe frame structure sometimes makes people feel confused. If there are many frames, there may be up and down, left and right scrollbars, which will distract visitor's attention and lead to poor user experience.
- The code is complex and can't be indexed by some search engines, which is very important.
- Many mobile devices can not fully display the frame, and the device compatibility is poor.
- Iframe framework pages will increase the HTTP requests of the server, which is not advisable for large websites.

## CSS

**#Question:** 简述你对 **_`BFC`_** 规范的理解

> [Block formatting context - Developer guides | MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)

A **Block formatting Context** is a part of a visual CSS rendering of a web page. It's the region in which the layout of block boxesx occurs and in which floats intreract with other elements.

A block formatting context is created bt at least one of the following:

- The root element of the document ---- `<html>`

- Floats (elements where `float` _isn't_ `none`)

- Absolutely positioned elements (`postion` is `absolute` of `fixed`)

- Inline-blocks (`display: inline-block;`)

- Table cells (element with `display: table-cell`, which is the default for HTML table cell.)

### 是什么？

常见的`Formatting Context`有 BFC、IFC（行级格式化上下文），还有 GFC（网格布局格式化上下文）和 FFC（自适应格式化上下文）

`BFC`是 **_一个独立渲染区_** 只有`Block-level box`参与， 它规定了内部的`Block-level Box`如何布局，并且与这个区域外部毫不相干。

`BFC` 是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然。

### 解决了什么问题(有什么作用)：

- 解决了 盒子内外的元素布局的相互影响，干扰。
- 解决了 自适应两栏布局
- 阻止父元素 高度坍塌（子元素都为 float 时，子元素脱离文档流 父元素不能被子元素撑开）（参考下文规则 6）
- 可以阻止元素被浮动元素覆盖
- 可以包含浮动元素——清除内部浮动
- 分属于不同的 BFC 时可以阻止 margin 重叠

### 如何触发 `BFC`

常用的四种方法：

1. float `is not` none
2. position `is not` static and relative (is fixed or absolute)
3. overflow `is` auto scroll and hidden
4. display `is` table-cell or inline-block

### `BFC` 的 布局规则

1. 内部的 `Box` 会在垂直方向，一个接一个地放置。
2. `Box` 垂直方向的距离由 `margin` 决定。属于同一个 `BFC` 的两个相邻 `Box` 的 `margin` 会发生重叠
3. 每个元素的 `margin box` 的左边， 与包含块 `border box` 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
4. `BFC` 的区域不会与 `float box` 重叠。
5. `BFC` 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算 `BFC` 的高度时，浮动元素也参与计算， 相当于清除了内部浮动。

## JavaScript

**#Question:** 统计某一字符或字符串在另一个字符串中出现的次数

```js
function substrCount(str, target) {
  let count = 0;
  if (!target) return count;
  while (str.match(target)) {
    str = str.replace(target, '');
    count++;
  }
  return count;
}

console.log(strCount('abcdef abcdef a', 'abc'));
```

---

```js
function substrCount(str, target) {
  let count = 0;
  while (str.includes(target)) {
    const index = str.indexOf(target);
    count++;
    str = str.substring(index + target.length);
  }
  return count;
}
```

---

```js
function substrCount(str, target) {
  return str.split(target).length - 1;
}
```

## Reference

[What are the advantages and disadvantages of iframe? | Develop Paper](https://developpaper.com/what-are-the-advantages-and-disadvantages-of-iframe/)

[[布局概念] 关于 CSS-BFC 深入理解 - 掘金](https://juejin.cn/post/6844903476774830094)

[[Jelly College] CSS tutorial lesson8:How to understand BFC quickly - YouTube](https://www.youtube.com/watch?v=Onhu2DlmHAk)
