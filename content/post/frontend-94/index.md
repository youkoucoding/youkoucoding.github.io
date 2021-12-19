---
title: '毎日のフロントエンド  94'
date: 2021-12-19T15:17:29+09:00
description: frontend 每日一练
image: frontend-94-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十四日

## HTML

### **Question:** 使用 HTML5 需要遵守哪些设计原则

- 避免不必要的复杂性

html4

`<!DOCTYPE html PUBLIC "-//W3C/DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`

html5

`<!DOCTYPE html>`

html4

`<meta http-equiv="Content-Type" content="text/html; charset=utf-8">`

html5

`<meta charset="utf-8">`

- 支持已有内容

- 解决现实的问题
- 语义化

html5 新增了多个元素，其中包括：`section`、`article`、`aside`和`nav`，它们代表了一种新的内容模型给内容分区。

- 平稳退化

浏览器在遇到不识别的 type 值时，会将 type 的值解释为 `text`

```html
input type="number" input type="search" input type="range" input type="email"
input type="date" input type="url"
```

- 最终用户优先

## CSS

### **Question:** 为什么会出现浮动？在什么时候需要清除浮动

- 什么是浮动：我们在做布局的时候用到的一种技术，通过浮动可以让元素左右浮动，然后通过 margin 调整位置
- 工作原理：使元素脱离文档流，让元素可以左右浮动，直到遇到另一个浮动元素的边缘才停止。
- 带来的问题：浮动元素会造成父级元素无法自动获取高度，导致父级塌陷，布局错乱。

## JavaScript

### **Question:** 用 js 写出死循环的方法有哪些

```js
while (true) {}

for (let i = 0; i > 0; i++) {}

// 这个等价于 while
for (;;) {}

let i = 0;
do {
  i++;
} while (i > 0);

// 不设置结束条件
function fn(a) {
  console.log(a);
  fn(a);
}
```

### **Question:** 将 `153812.7` 转化为 `153,812.7`

```js
function parseMoney(num) {
  let [interger, decimal] = String.prototype.split.call(num, '.');
  interger = interger.replace(/\d{1,3}(?=(\d{3})+$)/g, '$&,');
  decimal = decimal ? `.${decimal}` : '';
  return interger + decimal;
}
console.log(parseMoney(153812.7));
```

---

```js
function formatNum(num) {
  return new Intl.NumberFormat().format(num);
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[html5 设计原则-2007 - SegmentFault 思否](https://segmentfault.com/a/1190000019652601)

[HTML Design Principles](https://www.w3.org/TR/html-design-principles/)

[html5 需遵循的设计原则有哪些 - web 开发 - 亿速云](https://www.yisu.com/zixun/408759.html)

[Intl - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl)
