---
title: '毎日のフロントエンド　 76'
date: 2021-12-01T16:23:08+09:00
description: frontend 每日一练
image: frontend-76-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十六日

## HTML

### **Question:** `HTML5`如何识别语音读出的内容和朗读指定的内容

- [Web Speech API - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API)

- [SpeechSynthesis - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis) （实验性）

## CSS

### **Question:** 用`CSS`画出一个任意角度的扇形，可以写多种实现的方法

- 一个圆 + 两个绝对定位的半圆
- 扇形可以通过两个半圆作为遮罩旋转来露出相应的角度实现

```html
<div class="contain">
  <div class="main"></div>
  <div class="mask1 common"></div>
  <div class="mask2 common"></div>
</div>
```

```css
.contain {
  position: relative;
  width: 200px;
  height: 200px;
}
.main {
  height: 100%;
  background: lightgreen;
  border-radius: 100px;
}
.common {
  position: absolute;
  top: 0;
  width: 50%;
  height: 100%;
}
.mask1 {
  transform: rotate(83deg);
  border-radius: 100px 0 0 100px;
  left: 0;
  transform-origin: right center;
  background: red;
}
.mask2 {
  transform: rotate(-76deg);
  transform-origin: left center;
  left: 100px;
  border-radius: 0 100px 100px 0;
  background: blue;
}
```

---

Solution - 2 : `clip-path: polygon()`

## JavaScript

### **Question:** 说说`instanceof`和`typeof`的实现原理并模拟实现一个`instanceof`

#### `instacenof`

- 返回 `boolean`
- 通过调用 `class` 的 `[Symbol.hasInstance]` `static method` 来判断一个 `object` 是否是一个 `class` 的 `instance`
- 缺省行为：判断 `object` 的 `prototype chain` 中是否有任意一个 `prototype` 与 `class` 的 `prototype` 相等

```typescript
interface IConstructor<T> {
  new (...args: any[]): T;
}

function isObject(o: any) {
  return (typeof o === 'object' || typeof o === 'function') && o !== null;
}

function instanceOf<T>(obj: any, cls: IConstructor<T>): obj is T {
  if (isObject(cls)) {
    if (typeof cls[Symbol.hasInstance] === 'function')
      return cls[Symbol.hasInstance](obj);
    else if (typeof cls === 'function') {
      if (isObject(cls.prototype)) return cls.prototype.isPrototypeOf(obj);
      else return false;
    } else throw new TypeError('cls is not callable');
  } else throw new TypeError('cls is not an object');
}
```

#### `typeof`

- 返回 `string`, `number`, `undefined`, `boolean`, `object (null or Object)`, `function`, `symbol`

### **Question:** 选择正确的答案

```js
console.log([2, 1, 0].reduce(Math.pow));
console.log([].reduce(Math.pow));

/**
A. 2 报错
B. 2 NaN
C. 1 报错
D. 1 NaN
*/
```

Result: C: 1 error

- `arr.reduce(callback[, initialValue])`
  - `reduce` 接受两个参数, 一个回调, 一个初始值
  - 回调函数接受四个参数 `previousValue`, `currentValue`, `currentIndex`, `array`
  - If the array is empty and no initialValue was provided, TypeError would be thrown.
  - 所以第二个会报异常. 第一个表达式等价于 Math.pow(2, 1) => 2; Math.pow(2, 0) =>1

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
