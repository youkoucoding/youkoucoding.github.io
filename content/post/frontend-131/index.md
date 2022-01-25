---
title: '毎日のフロントエンド  131'
date: 2022-01-25T21:36:46+09:00
description: frontend 每日一练
image: frontend-131-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十一日

## HTML

### **Question:** table 去除边框的方法

- `border-style: hidden;`
- `border: 0;`
- `border: hidden;`
- `border-width: 0;`
- `border: transparent;`
- `border-color: transparent;` (仅为不可见)

```scss
table {
  border: none; // 去外边框
  td,
  th,
  tr {
    border: inherit; // 去内边框
  }
}
```

## CSS

### **Question:** 举例说明实现圆角的方式有哪些

```css
border-radius: 4px;
border-radius: 4px 8px; /* top-left,bottom-right; top-right, bottom-left */
border-radius: 4px 8px 4px; /* top-left; top-right, bottom-right; bottom-left */
border-radius: 4px 4px 8px 8px; /* top-left; top-right; bottom-right; bottom-left */
```

## JavaScript

### **Question:** JSON.stringify 有什么局限性

- `JSON.stringify()` 方法将一个 JavaScript 对象或值**转换为 `JSON` 字符串**，如果指定了一个 replacer 函数，则可以选择性地替换值，或者指定的 replacer 是数组，则可选择性地仅包含数组指定的属性。

- `JSON.stringify` 忽略`undefined`，`function`，`Symbol`，不能正确处理`NaN`，`Infinity`，**循环引用**(抛出异常 TypeError ("cyclic object value")（循环对象值）)

- `JSON.parse()` 方法用来**解析 `JSON` 字符串**，构造由字符串描述的 JavaScript 值或对象。提供可选的 reviver 函数用以在返回之前对所得到的对象执行变换(操作)。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[border-radius - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)

[JSON.stringify() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

[JSON.parse() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
