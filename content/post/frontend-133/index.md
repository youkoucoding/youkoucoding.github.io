---
title: '毎日のフロントエンド  133'
date: 2022-01-27T16:05:01+09:00
description: frontend 每日一练
image: frontend-133-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百三十三日

## HTML

### **Question:** canvas 默认画布的尺寸是多大？怎样设置才能不会变形

- `canvas`
  - `width`、`height` 属性，一般称其为 **画布尺寸**，即图形绘制的地方。默认值分别为 `300px`、`150px`
    - `<canvas id="canvas" width="300" height="150"></canvas>`
  - CSS 样式里的 width、height 属性，可通过内联样式、内部样式表或外部样式表设置。一般称其为 **画板尺寸**，用于渲染绘制完成的图形。默认值为空
    - `<canvas id="canvas" style="width: 300px; height: 150px;"></canvas>`

| 画布尺寸 | 画板尺寸 | 说明                           |
| -------- | -------- | ------------------------------ |
| 已设置   | 未设置   | 画板尺寸随画布尺寸改变         |
| 未设置   | 已设置   | 画板尺寸将不再随画布尺寸而改变 |
| 已设置   | 已设置   | 画板尺寸将不再随画布尺寸而改变 |

- **避免图形变形失真，要保持画布尺寸和画板尺寸一致**

```js
var canvas = document.getElementById('a');
canvas.width = '500';
canvas.height = '300';
```

## CSS

### **Question:** css3 实现一个 div 设置多张背景图片

```css
div {
  background-image: url('1.jpg'), url('2.jpg'), url('3.jpg'); /* 逗号分隔*/
  background-repeat: no-repeat, no-repeat, no-repeat;
  background-position: 0 0, 200px 0, 400px 201px;
}
```

## JavaScript

### **Question:** 将字符串中的单词倒转后输出

```js
const reverseStr = (str) => (str ? str.split('').reverse().join('') : str);

console.log(reverseStr('my love'));
console.log(reverseStr('abc cba abc'));
```

---

```js
const reverseStr = (str) => {
  if (typeof str !== 'string') throw new Error('The argument must be a string');
  return str
    .split(' ')
    .map((v) => v.split('').reverse().join(''))
    .join(' ');
};

console.log(reverseStr('my love'));
console.log(reverseStr('js upupup'));
console.log(reverseStr(123));
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[canvas - 画布尺寸](https://tsejx.github.io/visualization-guidebook/canvas/basic/scale)
