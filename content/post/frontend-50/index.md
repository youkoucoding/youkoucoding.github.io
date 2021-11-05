---
title: '毎日のフロントエンド　50'
date: 2021-11-05T11:24:58+09:00
description: frontend 每日一练
image: frontend-50-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十日

## CSS

### **Question:** 列举 CSS 优化、提高性能的方法

#### 加载性能

1. 压缩 CSS
2. 尽量，通过`link`方式加载，而不是`@import`
3. 复合属性其实分开写，执行效率更高，因为 CSS 最终也还是要去解析如 `margin-left: left;`

#### 选择器性能

1. 尽量少的使用嵌套，可以采用`BEM`[^1]的方式来解决命名冲突
2. 尽量少甚至是不使用标签选择器，这个性能实在是差，同样的还有\*选择器
3. 利用继承，减少代码量

[^1]: [BEM 101 | CSS-Tricks](https://css-tricks.com/bem-101/)

#### 渲染性能

1. 慎重使用高性能属性：浮动、定位；
2. 尽量减少页面重排、重绘；
3. css 雪碧图
4. 自定义 web 字体，尽量少用
5. 尽量减少使用昂贵属性，如 box-shadow/border-radius/filter/透明度/:nth-child 等
6. 使用 transform 来变换而不是宽高等会造成重绘的属性

## JavaScript

### **Question:** 请写出一个函数求出 N 的阶乘（即 N!）

```js
const factorial = (n) => {
  if (n > 1) return n * factorial(n - 1);
  return 1;
};
```

## One more question

写出执行结果，并解释原因

```js
var min = Math.min();
max = Math.max();
console.log(min < max);
```

- false
- MDN 相关文档
  - `Math.min` 的参数是 0 个或者多个，如果多个参数很容易理解，返回参数中最小的。如果没有参数，则返回 `Infinity`，无穷大。
  - `Math.max` 没有传递参数时返回的是`-Infinity`. 所以输出 `false`

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
