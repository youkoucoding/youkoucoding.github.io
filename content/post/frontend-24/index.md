---
title: '毎日のフロントエンド　24'
date: 2021-10-09T23:29:09+09:00
description: frontend 每日一练
image: frontend-24-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十四日

## HTML

### **#Question:** 说说你对属性 data-的理解

`data-*`[^1] 是`HTML5`新增的自定义属性，可以用来页面间跳转时携带数据

[^1]: [使用数据属性 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Howto/Use_data_attributes)

`data-*` 便是 `HTML5` 中用来存放数据的标签。使用 `data-*` 时，`data-` 之后的单词**必须是小写的**，但是可以用多个 - 连接。而在对应的 `DOM` 方法中，我们可以通过 `element.dataset[属性名]` 进行访问。在这里的属性名可以使用驼峰（转换规则和 `vue` 的组件名称转换一样）。

相比之前的自定义属性存放数据，使用 `data-*` 的方法，在数据的获取上会比较方便

## CSS

### **#Question:** 有用过 *CSS 预处理器*吗？喜欢用哪个？原理是什么？

`CSS`预处理器 可使`CSS`具备更加简洁、适应性更强、可读性更强、层级关系更加明显、更易于代码的维护等诸多好处。
CSS 预处理器种类繁多，目前`Sass`、`Less`、用的比较多:

1. 嵌套：反映层级和约束
2. 变量和计算： 减少重复代码
3. `Extend` 和 `Mixin` 代码片段 (用的少)
4. 循环：适用于复杂有规律的样式
5. `import css` 文件模块化

## JavaScript

### **#Question:** 如何快速让一个数组乱序，写出来

> 使用`array.sort()`进行乱序存在一定问题，增大样本进行实验之后可以发现这种乱序方案并不是完全随机的（所有元素会大概率停留在自己的初始位置）（v8 处理排序是小于 10 个是插入排序，大于 10 个是快排，排序算法复杂度介于 O(n)与 O(n2)之间，也就是存在两个元素都没有比较的机会，因此不是完全随机），这里可以使用**Fisher–Yates shuffle**（洗牌算法）

```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
arr.map((item, index) => {
  let random = Math.floor(Math.random() * arr.length);
  [arr[index], arr[random]] = [arr[random], arr[index]];
});
console.log(arr);

// or
// return a new Array
const swap = (arr, index1, index2) => {
  [arr[index1], arr[index2]] = [arr[index2], arr[index1]];
};

function shuffle(arr = []) {
  if (!Array.isArray(arr)) return;
  let index = arr.length - 1;
  for (; index >= 0; index--) {
    const randomIndex = Math.floor(Math.random() * (index + 1));
    swap(arr, index, randomIndex);
  }
}
```

补充知识： js `AST`[^2]

[^2]: [ AST 抽象语法树——最基础的 javascript 重点知识 ](https://segmentfault.com/a/1190000016231512)

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
