---
title: '毎日のフロントエンド　27'
date: 2021-10-13T11:09:57+09:00
description: frontend 每日一练
image: frontend-27-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十七日

## HTML

### **#Question:** 说说对影子`(Shadow)DOM` 的了解

**Shadow DOM**: A set of JavaScript APIs for attaching an encapsulated "shadow" DOM tree to an element --- which is rendered separately from the main document DOM --- and controlling associated functionality.In this way, you can keep an element's features private, so they can be scripted and styled without the fear of collision with other parts of the document.

![shadowdom](shadowdom.svg)

There are some bits of shadow DOM terminology to be aware of:

- **Shadow host**: The regular DOM node that the shadow DOM is attached to.
- **Shadow tree**: The DOM tree inside the shadow DOM
- **Shadow boundary**: The place where the shadow DOM ends, and the regular DOM begins
- **Shadow root**: the root node of shadow tree

`shadow Dom`是`html`给出的一个用来封装的虚拟 DOM 与普通的 DOM 不相同，他更像伪类元素，去修饰 DOM，或者说，他是一个 DOM 的 HTML 组件，常见标签为 video 等媒体标签（这些已经封装好的标签，有对应样式）

## CSS

### **#Question:** 怎样修改 chrome 记住密码后自动填充表单的黄色背景

当记住用户名和密码后，下次填写表单时，被记住的部分会被填充为淡黄色

```css
input {
  &:-webkit-autofill {
    box-shadow: 0 0 0px 1000px rgba(255, 255, 255, 0) inset !important;
    -webkit-text-fill-color: #000 !important;
    transition: background-color 5000s ease-in-out 0s;
    font-size: 14px;
  }
}
```

## JavaScript

### **#Question:** 说说对`arguments`的理解，它是数组吗

#### THE arguments object

`arguments` is an `Array`-like object accessible inside `functions` that contains the values of the arguments passed to that function.

> ES6 compatible code, the `rest parameters` should be preferred.

> "Array-like" means that `arguments` has a `length` property and properties indexed from zero, but it doesn't have `Array`'s built-in methods like `forEach()` or `map()`.

`arguments`是一个对象。

`js`不能像`Java`一样实现重载，`arguments` 对象可以模拟重载。

`js`中每个函数都会有`arguments`这个实例，它引用着函数的实参，可以用数组下标的方式"`[]`"引用`arguments`的元素。`arguments.length`为函数实参个数，`arguments.callee`引用函数自身。

Feature：

1. `arguments`对象和`Function`是分不开的。

2. 因为`arguments`这个对象不能显式创建。

3. `arguments`对象只有函数开始时才可用。

Usage：

虽然`arguments`对象并不是一个数组，但是访问单个参数的方式与访问数组元素的方式相同: `arguments[0], arguments[1]...`

## Reference

[Shadow DOM concepts - Polymer Project](https://polymer-library.polymer-project.org/2.0/docs/devguide/shadow-dom)

[Web Components | MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

[The arguments object - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)
