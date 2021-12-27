---
title: '毎日のフロントエンド  102'
date: 2021-12-27T20:20:51+09:00
description: frontend 每日一练
image: frontend-102-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零二日

## HTML

### **Question:** 行内元素、块级元素、空(void)元素分别有哪些

#### 行内元素

- `a`, `b`, `input`, `span`, `label`, `button`, `textarea`...

#### 块级元素

- `div`, `form`, `table`, `h1`...`h6`, `p`

#### 空元素

- `img`, `input`, `hr`, `br`, `meta`, `source`,`track`

## CSS

### **Question:** 如何规划响应式布局的

Mobile 优先： 先已较少的 css 代码实现相对简单移动端页面。

## JavaScript

### **Question:** `'1,2,3,4'.split()`的结果是什么（包括类型和值）

Result: `["1,2,3,4"]`

- 一个长度为 1 的`Array`，元素类型为`String`
- `split`，其可以接受两个参数:
  1. 第一个参数是字符串或正则表达式，从该参数指定的地方分割 stringObject；
  2. 第二个参数, 限制返回 Array 的最大长度
- `split()`，默认将整个字符串作为一个元素返回一个长度为 1 的 `Array`

### **Question:** 以下代码执行的结果

```js
var obj = {
  2: 3,
  3: 4,
  length: 2,
  splice: Array.prototype.splice,
  push: Array.prototype.push,
};
obj.push(1);
obj.push(2);
console.log(obj);
```

Result: `[empty × 2, 1, 2, splice: ƒ, push: ƒ]` **类数组**

- 在对象中加入 splice 属性方法，和 length 属性后。这个对象变成一个**类数组**。

1. 第一次`push`，`obj`对象的`push`方法设置 `obj[2]=1;` **`obj.length+=1;`**
2. 第二次`push`，`obj`对象的`push`方法设置 `obj[3]=2;` **`obj.length+=1`**
3. `console.log()`: `obj`具有 `length` 属性和 `splice` 方法，故将其作为数组进行打印 4.打印时因为数组未设置下标为 0 1 处的值，故打印为 empty，主动 obj[0] 获取为 undefined

#### 类数组（`ArrayLike`）

一组数据，由数组来存，但是如果要对这组数据进行扩展，会影响到数组原型，ArrayLike 的出现则提供了一个中间数据桥梁，ArrayLike 有数组的特性， 但是对 ArrayLike 的扩展并不会影响到原生的数组。

#### `push`方法[^1]：

push 方法有意具有通用性。该方法和 `call()` 或 `apply()` 一起使用时，可应用在**类数组的对象**上。**`push` 方法根据 `length` 属性来决定从哪里开始插入给定的值。** 如果 length 不能被转成一个数值，则插入的元素索引为 0，包括 length 不存在时。当 length 不存在时，将会创建它。 唯一的原生类数组（array-like）对象是 Strings，尽管如此，它们并不适用该方法，因为字符串是不可改变的。

---

`Array.prototype.slice()`[^2]

[^1]: [Array.prototype.push() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push#description)
[^2]: [Array.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[前端响应式布局原理与方案（详细版） - 掘金](https://juejin.cn/post/6844903814332432397)
