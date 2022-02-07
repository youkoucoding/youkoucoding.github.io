---
title: '毎日のフロントエンド  144'
date: 2022-02-07T11:54:29+09:00
description: frontend 每日一练
image: frontend-144-cover.jpg
categories:
  - Javascript Tips
---

# 第一百四十四日

## CSS

### **Question:** `background-size` `background-origin` `background-clip`

#### `background-size`

- 控制背景图片大小

1. `background-size: cover`
   - 缩放背景图片以完全覆盖背景区，可能背景图片部分看不见。和 contain 值相反，cover 值尽可能大的缩放背景图像并**保持图像的宽高比例**（图像不会被压扁）。该背景图以它的全部宽或者高覆盖所在容器。当容器和背景图大小不同时，背景图的 左/右 或者 上/下 部分会**被裁剪**
2. `background-size: contain`
   - 缩放背景图片以完全装入背景区，可能背景区部分空白。contain 尽可能的缩放背景并保持图像的宽高比例（图像不会被压缩）。该背景图会填充所在的容器。当背景图和容器的大小的不同时，容器的空白区域（上/下或者左/右）会显示**由 background-color 设置的背景颜色**

#### `background-origin`

- 规定了指定背景图片 background-image 属性的原点位置的背景相对区域、控制图片平铺位置

1. `background-origin: content-box`
   - 背景图从内容开始平铺，而不从 padding 开始
2. `background-origin: border-box`
   - 背景图从 border 开始平铺
3. `padding-box` 默认值

#### `background-clip`

- 控制图片的平铺后裁切掉只展示哪些区域
- `background-origin`控制的是平铺图片的起点，`background-clip`控制的是平铺图片后裁切要展示哪些区域
- `background-clip` 会影响到是图片和颜色，而 `background-origin` 不能影响颜色，只能控制图片

1. `background-clip: padding-box`
   - 从 padding 到 content 的部分才展示
2. `background-clip: content-box`
   - border 和 padding 不会展示出背景图和颜色
3. `border-box`默认值

## JavaScript

### Tips: 数组方法 (改变自身和不改变自身)

#### 改变自身的方法

基于 ES6，会改变自身值的方法一共有 9 个，分别为 `pop`、`push`、`reverse`、`shift`、`sort`、`splice`、`unshift`，以及两个 ES6 新增的方法 `copyWithin` 和 `fill`

```js
// pop方法
var array = ['cat', 'dog', 'cow', 'chicken', 'mouse'];
var item = array.pop();
console.log(array); // ["cat", "dog", "cow", "chicken"]
console.log(item); // mouse
// push方法
var array = ['football', 'basketball', 'badminton'];
var i = array.push('golfball');
console.log(array);
// ["football", "basketball", "badminton", "golfball"]
console.log(i); // 4
// reverse方法
var array = [1, 2, 3, 4, 5];
var array2 = array.reverse();
console.log(array); // [5,4,3,2,1]
console.log(array2 === array); // true
// shift方法
var array = [1, 2, 3, 4, 5];
var item = array.shift();
console.log(array); // [2,3,4,5]
console.log(item); // 1
// unshift方法
var array = ['red', 'green', 'blue'];
var length = array.unshift('yellow');
console.log(array); // ["yellow", "red", "green", "blue"]
console.log(length); // 4
// sort方法
var array = ['apple', 'Boy', 'Cat', 'dog'];
var array2 = array.sort();
console.log(array); // ["Boy", "Cat", "apple", "dog"]
console.log(array2 == array); // true
// splice方法
var array = ['apple', 'boy'];
var splices = array.splice(1, 1);
console.log(array); // ["apple"]
console.log(splices); // ["boy"]
// copyWithin方法
var array = [1, 2, 3, 4, 5];
var array2 = array.copyWithin(0, 3);
console.log(array === array2, array2); // true [4, 5, 3, 4, 5]
// fill方法
var array = [1, 2, 3, 4, 5];
var array2 = array.fill(10, 0, 3);
console.log(array === array2, array2);
// true [10, 10, 10, 4, 5], 可见数组区间[0,3]的元素全部替换为10
```

#### 不改变自身的方法

- 基于 ES7，不会改变自身的方法也有 9 个，分别为 `concat`、`join`、`slice`、`toString`、`toLocaleString`、`indexOf`、`lastIndexOf`、未形成标准的 toSource，以及 ES7 新增的方法 `includes`

```js
// concat方法
var array = [1, 2, 3];
var array2 = array.concat(4, [5, 6], [7, 8, 9]);
console.log(array2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(array); // [1, 2, 3], 可见原数组并未被修改
// join方法
var array = ['We', 'are', 'huaman being'];
console.log(array.join()); // "We,are,human being"
console.log(array.join('+')); // "We+are+human being"
// slice方法
var array = ['one', 'two', 'three', 'four', 'five'];
console.log(array.slice()); // ["one", "two", "three","four", "five"]
console.log(array.slice(2, 3)); // ["three"]
// toString方法
var array = ['Jan', 'Feb', 'Mar', 'Apr'];
var str = array.toString();
console.log(str); // Jan,Feb,Mar,Apr
// tolocalString方法
var array = [{ name: 'zz' }, 123, 'abc', new Date()];
var str = array.toLocaleString();
console.log(str); // [object Object],123,abc,2016/1/5 下午1:06:23
// indexOf方法
var array = ['abc', 'def', 'ghi', '123'];
console.log(array.indexOf('def')); // 1
// includes方法
var array = [-0, 1, 2];
console.log(array.includes(+0)); // true
console.log(array.includes(1)); // true
var array = [NaN];
console.log(array.includes(NaN)); // true
```

- **slice 不改变自身，而 splice 会改变自身**

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/question/)

[background-size - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size)

[background-origin - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-origin)

[background-clip - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

[理解 background-origin、background-clip、background-size](https://juejin.cn/post/6844904094910398477)
