---
title: '毎日のフロントエンド  178'
date: 2022-03-16T12:33:45+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `requestAnimationFrame`

- `requestAnimationFrame` 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率
- 运用场景：
  - js 动画：requestAnimationFrame 是由浏览器专门为动画提供的 API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了 CPU 开销
  - 大数据渲染：在大数据渲染过程中，比如将后台返回的十万条记录插入到表格中，如果一次性在循环中生成 DOM 元素，会导致页面卡顿，用户体验差。这时候就可以用 `requestAnimationFrame` 进行分步渲染，确定最好的时间间隔，使得页面加载过程中很流畅

## CSS

### css 有哪些不可继承的属性

- 在 css 中，每个 CSS 属性定义的概述都指出了这个属性是默认继承的("Inherited: Yes") 还是默认不继承的("Inherited: no")。这决定了当你没有为元素的属性指定值时该如何计算值。

- [Full property table](https://www.w3.org/TR/CSS21/propidx.html)

## JavaScript

### TailCall 尾调用

> 对递归的优化一般有两个思路，减少递归次数和使用**尾调用**，亦或改为迭代
> 目前大部分浏览器没有对尾递归（尾调用）做优化(safari 除外)，依然会导致栈溢出

- 尾调用（`Tail Call`）是指函数的最后一步返回另一个函数的调用。
- 尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用记录，只要直接用内层函数的调用记录取代外层函数的调用记录就可以了，调用栈中始终只保持了一条调用帧。
- 尾调用优化（Tail call optimization），即只保留内层函数的调用帧。如果所有的函数都是尾调用的话，调用位置、内部变量等信息都不会再用到了，那么在调用栈中的调用帧始终只有一条，这样会节省很大一部分的内存，这也是尾调用优化的意义。
- 只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。

```js
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
// =======
f();
// 等同于
g(3);
```

```js
// 函数 a() 返回了函数 b() 的调用
function a(x) {
  return b(x);
}

// 返回缓存的函数调用结果，或者返回多个函数调用都不属于“尾调用”
function a(x) {
  let c = b(x);
  return c;
}
function a(x) {
  return b(x) + c(x);
}
function a() {
  b(x);
}
```

---

```js
const a = () => {
  b();
};
```

相当于

```js
const a = () => {
  b();
  return undefined;
};
```

- 不属于 Tail Call

---

#### 尾递归

- 尾调用由于是在 return 语句中，并且是函数的最后一步操作，所以局部变量等信息不需要再用到，从而可以立即释放节省内存空间。
- 函数调用自身，称为递归。如果尾调用自身，就称为尾递归。递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。
- 尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是**把所有用到的内部变量改写成函数的参数。**

```js
function fib(n) {
  if (n < 3) return 1;
  return fib(n - 1) + fib(n - 2);
}

// 尾递归调用， 采用es6 的函数默认值方法
function fibTail(n, a = 0, b = 1) {
  if (n === 0) return a;
  return fibTail(n - 1, b, a + b);
}
```

方法二

```js
function currying(fn, n) {
  return function (m) {
    return fn.call(this, m, n);
  };
}

function tailFactorial(n, total) {
  if (n === 1) return total;
  return tailFactorial(n - 1, n * total);
}

const factorial = currying(tailFactorial, 1);

factorial(5); // 120
```

---

- 尾调用不一定出现在函数尾部，只要是最后一步操作即可。
- ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

---

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[window.requestAnimationFrame - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

[Full property table](https://www.w3.org/TR/CSS21/propidx.html)

[继承 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inheritance)

[ES6 尾调用和尾递归](https://segmentfault.com/a/1190000020694801)
