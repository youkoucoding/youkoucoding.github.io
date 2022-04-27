---
title: '毎日のフロントエンド 217'
date: 2022-04-27T23:04:13+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## HTML

### 页面加载后，表单的第一个文本框如何自动获得焦点

1. `<input type="text" autofocus/>`

2. `<input id="input" type="text"/> document.getElementById('input').focus();`

## CSS

### `:not()`的使用场景

- `:not()` 用来匹配不符合一组选择器的元素。由于它的作用是防止特定的元素被选中，它也被称为反选伪类（negation pseudo-class）

```css
/* 子级之间留 10px 空隙 */
.gap-right-10 > :not(:last-child) {
  margin-right: 10px;
}

/* 有数据时加上标题 */
.list-wrap:not(:empty):before {
  content: attr(data-title);
}

/* flex 容器中都不压缩宽度 */
.flex-row {
  display: flex;
  align-items: center;
  & > .grow {
    flex-grow: 1;
  }
  & > :not(.grow) {
    flex-shrink: 0;
  }
}
```

## JavaSctript

### 举例说明 js 创建数组有哪些方法

```js
const arr = [...document.querySelectorAll(`[data-dom="^div"`)];

const arr = [...new Set()];

// (2) [Array(2), Array(2)]
const arr = [...new Map()];
const arrInit = [
  ...new Map([
    ['a', 1],
    ['b', 2],
  ]),
];

const arr = new Uint8Array();

const arr = Array.from(''); // []
const arrInit = Array.from('abc'); // (3) ["a", "b", "c"]

const arr = Object.keys({}); // []

const arr = Object.values({}); // []

const arr = Object.entries({}); // []
```

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTMLInputElement - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLInputElement)

- [`:not()` - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not)

- [`:empty` - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:empty)

- [Array.from() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

- [Object.keys() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

- [Object.values() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/values)

- [Object.entries() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
