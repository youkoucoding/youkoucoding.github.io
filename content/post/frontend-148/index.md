---
title: '毎日のフロントエンド  148'
date: 2022-02-11T16:42:52+09:00
description: frontend 每日一练
image: frontend-148-cover.jpg
categories:
  - HTML
  - CSS
  - JavaScript
---

# 第一百四十八日

## HTML

**Question:** input 的`onblur`和`onchange`事件区别是什么

- `onblur`在 input 失去焦点时候触发。与之对应的是 onfocus 事件。无论 input 是否有值、值是否有变化，都会触发
- `onchange`在 input **发生变化然后在失去焦点的时候触发**。且先于 `onblur` 触发, `onchange` 只有在 input 的值必须与前一次输入不同才会触发

## JavaScript

**Question:** 原生的 js 实现斐波那契数列

```js
const Fibonacci = (num) => {
  if (!Fibonacci.sequence) Fibonacci.sequence = [1, 1];
  if (Fibonacci.sequence[num] !== undefined) return Fibonacci.sequence.slice(0, num);
  for (let i = Fibonacci.sequence.length; i < num + 1; i++) {
    Fibonacci.sequence[i] = Fibonacci.sequence[i - 1] + Fibonacci.sequence[i - 2];
  }
  return Fibonacci.sequence.slice(0, num);
};
```

---

```js
function fibonacci(num) {
  var a = 1,
    b = 0,
    temp;

  while (num >= 0) {
    temp = a;
    a = a + b;
    b = temp;
    num--;
  }

  return b;
}
```

- Time complexity: `O(N)`
- Space complexity: `Constant`

---

```js
function fibonacci(num) {
  if (num <= 1) return 1;

  return fibonacci(num - 1) + fibonacci(num - 2);
}
```

- Time complexity: `O(2^N)`
- Space complexity: `O(n)`

---

```js
function fibonacci(num, memo = {}) {
  if (memo[num]) return memo[num];
  if (num <= 1) return 1;

  return (memo[num] = fibonacci(num - 1, memo) + fibonacci(num - 2, memo));
}
```

- Time complexity: `O(2N)`
- Space complexity: `O(n)`

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Fibonacci sequence algorithm in Javascript | by devlucky | Developers Writing | Medium](https://medium.com/developers-writing/fibonacci-sequence-algorithm-in-javascript-b253dc7e320e)
