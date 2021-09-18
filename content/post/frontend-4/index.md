---
title: '毎日のフロントエンド　4'
date: 2021-09-18T13:33:07+09:00
description: frontend 每日一练
image: frontend-1-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四天

## HTML

**#Question:** HTML5 的文件离线储存怎么使用，工作原理是什么？

TL;DR -- `HTML5` local storage is an alternative to `cookies`, allowing web applications to store user information in their browser.

With web storagem web applications can store data locally within the user's browser.

Before HTML5, application data had to be stored in cookies, included in every server request. Web storage is more secure, and large amounts of data can be stored locally, without affecting website performance.

Web storage is per origin (per domain and protocal). All pages, from one origin, can store and access the same data.

- `window.localStorage` - stores data with no expiration date.

- `window.sessionStorage` - stores data for one session (data is lost when the browser tab is closed)

#### Cookies VS. Local Storage VS. Session Storage

|                   | Cookies            | Local Storage | Session Storage |
| ----------------- | ------------------ | ------------- | --------------- |
| Capacity          | 4kb                | 10mb          | 5mb             |
| Browsers          | HTML4/HTML5        | HTML5         | HTML5           |
| Accessible form   | Any window         | Any window    | Same tab        |
| Expires           | Manually set       | Never         | On tab close    |
| Storage Location  | Browser and Server | Browser only  | Browser only    |
| Sent with Request | Yes                | No            | No              |

[Storage Inspector - Firefox Developer Tools | MDN](https://developer.mozilla.org/en-US/docs/Tools/Storage_Inspector)

## CSS

**#Question:** CSS 选择器有哪些？哪些属性可以继承？

[CSS selectors - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

**_`CSS Selectors`_**

- **`Basic selectors`:**

  - `universal selector`通用选择器： `*` will match all the elements of the document.
  - `Type selector` 元素选择器： Syntax: `element name` input -> `<input>`
  - `Class selector` 类选择器具： Syntax: `.classname`, `.index` will match any element that has a class of "index".
  - `ID selector`： Syntax: `#idname`, `#toc` will match the element that has the ID "toc".
  - `Attribute selector` 属性选择器： Syntax: ` [attr]``[attr=value] `

- **`Grouping selectors`:**

  - `Selector list`: Syntax: A, B Example: `div`, `span` will match both `<span>` and `<div>` elements.

- **`Combinators`:** 组合器

  - `Descendant combinator` 后代组合器
  - `Child combinator` 直接子代组合器
  - `General sibling combinator` 一般兄弟组合器
  - `Adjacent sibling combinator` 紧邻组合器
  - `Column combinator` 列组合器

- **`Pseudo`:**

  - `Pseudo classes` 伪类： `:` The `:` pseudo allow the selection of elements based on state information that is not contained in the document tree. _Example_: `a::visited` will match all `<a>` elements that have been visited by the user.
  - `Psendo elements` 伪元素： The `::` pseudo represent entities that are not included in HTML. _Example_: `p::first-line` will match the first line of all `<p>` elements.

**_Inherit : yes_** 可继承属性：

[css 有哪些属性可以继承？](https://www.jianshu.com/p/fbfc6c751e34)
[层叠与继承 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)

#### **`Cascade`**

Stylesheets cascade - at a very simple level, this means that the order of CSS rules matter; whjen two rules apply that have equal specificity the one that comes last in the CSS is the one that will be used.

当两条同样级别的规则应用到同一个元素上时， 生效的是 后一个。

#### **`Inheritance`**

Some css property values set on parent elements are inherited by their child elements, and some aren't.

##### 可继承的`CSS``属性：

1. 字体相关： `font` `font-family` ......
2. 文本相关属性： `text-indent`, `text--align`, `color`......
3. 元素可见性： `visibility`
4. 表格布局属性： `caption-side, border-collapse, border-spacing,empty-cells, table-layout`
5. 列表属性： `list-style-type` ......
6. 生成内容属性： `quotes`
7. 光标属性： `cursor`
8. 页面样式属性： `page, page-break-inside, windows, orphans`......
9. 声音样式属性： `speak`......

##### 无继承性的属性

1. `display`
2. 文本属性： `vertical-align` `text-decoration` `text-shadow` `white-space` `unicode-bidi`
3. 盒子模型的属性:宽度、高度、内外边距、边框等
4. 背景属性：背景图片、颜色、位置等
5. 定位属性：浮动、清除浮动、定位 `position` 等
6. 生成内容属性: `content` `counter-reset` `counter-increment`
7. 轮廓样式属性: `outline-style` `outline-width` `outline-color` `outline`
8. 页面样式属性: `size` `page-break-before` `page-break-after`

##### 继承中比较特殊的几点

1. `a` 标签的字体颜色不能被继承

2. `h1-h6` 标签字体的大下也是不能被继承的 ,因为它们都有一个默认值

## JavaScript

**#Question:** 写一个方法把下划线命名转成大驼峰命名

```js
function toCamel(str) {
  str = str.replace(/(\w)/, (match, $1) => `${$1.toUpperCase()}`);
  while (str.match(/\w_\w/)) {
    str = str.replace(
      /(\w)(_)(\w)/,
      (match, $1, $2, $3) => `${$1}${$3.toUpperCase()}`
    );
  }
  return str;
}

console.log(toCamel('a_c_def')); // ACDef
```

```js
function toCamelCase(str) {
  if (typeof str !== 'string') {
    return str;
  }
  return str
    .split('_')
    .filter((s) => !!s) // 验证是否存在
    .map((item) => item.charAt(0).toUpperCase() + item.substr(1, item.length))
    .join('');
}
```
