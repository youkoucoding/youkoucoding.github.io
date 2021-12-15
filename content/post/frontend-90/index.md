---
title: '毎日のフロントエンド  90'
date: 2021-12-15T15:51:17+09:00
description: frontend 每日一练
image: frontend-90-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十日

## HTML

### **Question:** `ol`和`ul`标签的区别？它们的运用场景分别是什么

`ol` 为*有序列表*，`ul` 为*无需列表*。浏览器默认会给这两个加上不同的样式。可以通过 `list-style-type` 来改变样式。

- `ol` 的列表前会添加序号
- `ul` 的列表前则是圆点

`ol` 和 `ul` 有语义上的区别，对于确实有顺序关系的我们应该使用 `ul`。

## CSS

### **Question:** CSS 的`overflow`属性定义溢出元素内容区的内容会如何处理

- `visible`（默认值）：溢出的内容会照常显示在元素内容区之外；
- `hidden`：溢出的内容会被裁剪；
- `scroll`：溢出的内容会出现在滚动区，通过滚动条滚动可以看到；
- `auto`：当内容溢出时表现同`scroll`；

> 除了 `visible` 之外，其他的属性都会触发 `BFC`。`overflow:hidden` 也常常被用来进去浮动的清除。

## JavaScript

### **Question:** 写个方法随机打乱一个数组

> [毎日のフロントエンド　 24](https://youkoucoding.github.io/p/%E6%AF%8E%E6%97%A5%E3%81%AE%E3%83%95%E3%83%AD%E3%83%B3%E3%83%88%E3%82%A8%E3%83%B3%E3%83%89-24/#question-%E5%A6%82%E4%BD%95%E5%BF%AB%E9%80%9F%E8%AE%A9%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84%E4%B9%B1%E5%BA%8F%E5%86%99%E5%87%BA%E6%9D%A5)

```js
function shuffle(arr) {
  arr.sort(() => Math.random() - 0.5);
}
```

以上方法不能真正随机打乱数组。

Fisher–Yates shuffle 算法

```js
function shuffle(arr) {
  let i = arr.length;
  while (i) {
    let j = Math.floor(Math.random() * i--);
    [arr[j], arr[i]] = [arr[i], arr[j]];
  }
}
```

### **Question:** 写出结果，并解释原因

```js
var a = { k1: 1 };
var b = a;
a.k3 = a = { k2: 2 };
console.log(a); // ?
console.log(b); // ?
```

Result:

```js
{k2:2}
{k1:1,k3:{k2:2}}
```

key:

1. `.`优先级 大于 `=`
2. 对象是引用类型，每个新对象都是一个新的内存地址。

- `var b = a;` a&b 指向同一处内存地址
- `a.k3 = a = { k2: 2 }` `.`运算符优先级高，因此先执行`a.k3`， 于是 a 和 b 在`.`执行完之后都是`{k1:1, k3:undefined}`
- `=` 从右向左执行，`a = { k2: 2 }` 于是 a 指向一个新的对象地址。
- 接下来执行`a.k3 = a`： 此时 `a.k3` 是最早执行的， 已经将最开始的 a 和 b 指向的那个原始对象修改为`{k1:1, k3:undefined}`， 因此 可以将 `a.k3 = a` 视为 `{k1:1, k3:undefined}.k3 = {k2:2}`

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[overflow - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow)

[关于 JavaScript 的数组随机排序 - oldj's blog](https://oldj.net/article/2017/01/23/shuffle-an-array-in-javascript/#comment-1466)

[Fisher–Yates shuffle - Wikipedia](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
