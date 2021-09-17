---
title: '毎日のフロントエンド　3'
date: 2021-09-16T17:39:42+09:00
description: frontend 每日一练
image: frontend-1-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三天

## HTML

**#Question:** `HTML` 全局属性`(global attribute)`有哪些（包含`HTML5`）?

> [Global attributes - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

**`Global attributes`** are attributes common to **all** `HTML` elements; they can be used on all elements, though they may have no effect on some elements.

**`document.body.__proto__`**

- `accesskey`: 生成键盘快捷键提示。Provides a hint for generating a keyboard shortcut for the current element.
- `autocapitalize`: 控制用户的文本输入 是否 如何 自动大写。 Controls whether and how text input is automatically capitalized as it is entered/edited by user.
- `autofocus`: 标识一个元素是否在页面加载时候聚焦。 Indicates that an element is to be focused on page load, or as soon as the `<dialog>` it is part of is displayed. Boolean, initially false.
- `class`: 一个以空格分隔的元素的类名（`classes` ）列表，它允许 CSS 和 Javascript 通过类选择器 (`class selectors`) 或 DOM 方法( `document.getElementsByClassName`)来选择和访问特定的元素。
- `contenteditable`: 指定元素内容是否可编辑。 An enumerated attribute indicating if the element should be editable by the user.
- `data-*`: 一类自定义数据属性，它赋予我们在所有 HTML 元素上嵌入自定义数据属性的能力。 Forms a class of attributes, called custom data attributes, that allow proprietary information to be exchanged between `HTML` and its `DOM` representation that may be used by scripts.
- `dir`: 设置元素文本方向（默认 ltr；rtl）。An enumberated attribute indicating the directionality of the element's text. It can have the following values.
- `draggable`: 设置元素是否可拖拽。 An enumerated attribute indicating whether the element can be dragged, using the `Drag and Drop API`.
- `enterkeyhint`: Hints what action label (or icon) to present for the enter key on virtual keyboards.
- `hidden`: 布尔属性表示该元素尚未或不再相关。例如，它可用于隐藏在登录过程完成之前无法使用的页面元素。A Boolean attribute indicates that the element is not yet, or is no longer, relevant. For example, it can be used to hide elements of the page tha can't be used until the login process has been completed. The broweser won't render such elements. This attribute must not be used to hide content that could legitimately be shown.
- `id`: 元素 id，文档内唯一。 Defines a uniwue identifier which must be unique in the whole document. Its purpose is to identify the element when linking (using a fragment identifier), scripting, or styling(with CSS).
- `inputmode`: 向浏览器提供有关在编辑此元素或其内容时要使用的虚拟键盘配置类型的提示。主要用于 `<input>`元素。 Provides a hint to browesers as to the type of virtual keyboard configuration to use when editing this element or its contents. Used primarily on `<input>` elements.
- `is`, `itemid`, `itemprop`, `itemref`, `itemscope`, `itemtype`, `lang`, `part`, `slot`, `spellcheck`, `style`, `tabindex`, `title`, `translate`

## CSS

**#Question:** 在页面上隐藏元素的方法有哪些？

占位：

- `visibility: hidden;` 页面会渲染只是不限显示
- `margin-left: -100%;`
- `opacity: 0;`
- `transform: scale(0);`
- `z-index: -9999;` (置于最下层)
- `ransform: skew(90deg, -90deg);`

不占位：

- `display: none;` 页面不会渲染，可以减少首屏渲染的时间，但是会引起回流和重绘
- `width: 0; height:0; overflow: hidden;`

仅针对 块内*文本*元素：

- `text-indent: -9999px;`
- `font-size: 0;`

## JAVASCRIPT

**#Question:** 去除字符串中最后一个指定的字符

```js
// regExp
function deleteLastStr(str, target) {
  if (!target || typeof str !== 'string' || typeof target !== 'string')
    return str;
  let reg = new RegExp(`${target}(?=([^${target}]*)$)`);
  return str.replace(reg, '');
}
```

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
