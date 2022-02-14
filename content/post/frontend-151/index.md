---
title: '毎日のフロントエンド  151'
date: 2022-02-14T10:24:45+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

# 第一百五十一日

## HTML

### **Question:** 在页面中添加数学公式

- [KaTeX – The fastest math typesetting library for the web](https://katex.org/)
- [MathJax | Beautiful math in all browsers.](https://www.mathjax.org/)

## Tips

### 常用的数组方法底层实现

#### `push` 方法的底层实现

```js
Array.prototype.push = function (...items) {
  let O = Object(this); // ecma 中提到的先转换为对象
  // >>> 无符号右移 >>> 0 对非负数 取整;
  // 确保 ensure that the length property is a unsigned 32-bit integer.
  let len = this.length >>> 0;
  let argCount = items.length >>> 0;
  // 2 ^ 53 - 1 为JS能表示的最大正整数
  if (len + argCount > 2 ** 53 - 1) {
    throw new TypeError('The number of array is over the max value');
  }
  for (let i = 0; i < argCount; i++) {
    O[len + i] = items[i];
  }
  let newLength = len + argCount;
  O.length = newLength;
  return newLength;
};
```

#### `pop`方法的底层实现

```js
Array.prototype.pop = function () {
  let O = Object(this);
  let len = this.length >>> 0;
  if (len === 0) {
    O.length = 0;
    return undefined;
  }
  len--;
  let value = O[len];
  delete O[len];
  O.length = len;
  return value;
};
```

#### `map`方法的底层实现

```js
Array.prototype.map = function (callbackFn, thisArg) {
  if (this === null || this === undefined) {
    throw new TypeError("Cannot read property 'map' of null");
  }
  if (Object.prototype.toString.call(callbackfn) != '[object Function]') {
    throw new TypeError(callbackfn + ' is not a function');
  }
  let O = Object(this);
  let T = thisArg;
  let len = O.length >>> 0;
  let A = new Array(len);
  for (let k = 0; k < len; k++) {
    if (k in O) {
      let kValue = O[k];
      // 依次传入this, 当前项，当前索引，整个数组
      let mappedValue = callbackfn.call(T, KValue, k, O);
      A[k] = mappedValue;
    }
  }
  // 新数组 A，并不改变原数组的值
  return A;
};
```

#### `reduce`方法的底层实现

```js
Array.prototype.reduce = function (callbackfn, initialValue) {
  // 异常处理，和 map 类似
  if (this === null || this === undefined) {
    throw new TypeError("Cannot read property 'reduce' of null");
  }
  // 处理回调类型异常
  if (Object.prototype.toString.call(callbackfn) != '[object Function]') {
    throw new TypeError(callbackfn + ' is not a function');
  }

  let O = Object(this);
  let len = O.length >>> 0;
  let k = 0;
  let accumulator = initialValue; // reduce方法第二个参数作为累加器的初始值

  // 初始值默认值不传的特殊处理；
  if (accumulator === undefined) {
    throw new Error('Each element of the array is empty');
    // 初始值不传的处理
    for (; k < len; k++) {
      if (k in O) {
        accumulator = O[k];
        k++;
        break;
      }
    }
  }
  for (; k < len; k++) {
    if (k in O) {
      // 注意 reduce 的核心累加器
      // 累加器以及 callbackfn 的处理逻辑
      accumulator = callbackfn.call(undefined, accumulator, O[k], O);
    }
  }
  return accumulator;
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[数学公式](http://jartto.wang/2018/07/08/perfect-math/)

[What is the JavaScript `>>>` operator and how do you use it? - Stack Overflow](https://stackoverflow.com/questions/1822350/what-is-the-javascript-operator-and-how-do-you-use-it)
