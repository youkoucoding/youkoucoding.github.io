---
title: '毎日のフロントエンド  139'
date: 2022-02-02T10:37:03+09:00
description: frontend 每日一练
image: frontend-139-cover.jpg
categories:
  - Javascript
---

# 第一百三十九日

## HTML

### **Question:** 让 textarea 标签中的内容原格式输出

- 方法一： 使用 `<pre>` 标签

- 方法二：使用 css `white-space`
  - `white-space`属性指定元素内的空白怎样处理
    - `normal` —— 默认，空白会被浏览器忽略
    - `pre` —— 空白会被浏览器保留。其行为方式类似 HTML 中的`<pre>`标签。
    - `nowrap` —— 文本不会换行，文本会在同一行继续，直到遇到`<br/>`标签为止。
    - `pre-warp` —— 保留空白符序列，但正常进行换行。
    - `pre-line` —— 合并空白符序列，但保留换行符
    - `inherit` —— 规定应该从父元素继承 `white-space` 属性的值

---

- `white-space:pre-warp` 输入多少空格都会输出的时候就带几个空格。
- `white-space:pre-line` 无论输入多少空格都会输出的时候就带几个空格，会合并为一个。

## CSS

### **Question:** background-color:transparent 和 opacity:0 的区别

- `background-color:transparents;` 对应的是背景色（background-color）设为透明；
- `opacity:0;` 对应是该元素透明度设为 0（该元素所有内容）

## JavaScript

### **Question:** 柯里化函数(currying)的理解

柯里化（Currying）是一种关于函数的高阶技术。它不仅被用于 JavaScript，还被用于其他编程语言。柯里化是一种函数的转换，它是指将一个函数从可调用的 `f(a, b, c)` 转换为可调用的 `f(a)(b)(c)`。**柯里化不会调用函数。它只是对函数进行转换。**

```js
function curry(f) {
  // curry(f) 执行柯里化转换
  return function (a) {
    return function (b) {
      return f(a, b);
    };
  };
}

// 用法
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert(curriedSum(1)(2)); // 3
```

- curry(func) 的结果就是一个包装器 function(a)
- 当它被像 `curriedSum(1)` 这样调用时，它的参数会被保存在词法环境中，然后返回一个新的包装器 `function(b)`
- 然后这个包装器被以 2 为参数调用，并且，它将该调用传递给原始的 sum 函数。

---

柯里化更高级的实现，例如 lodash 库的 [`_.curry`](https://lodash.com/docs/4.17.15#curry)

---

#### currying 好处

例子：

一个用于格式化和输出信息的日志（logging）函数 `log(date, importance, message)`。在实际项目中，此类函数具有很多有用的功能，例如通过网络发送日志（log），在这儿我们仅使用 alert：

```js
function log(date, importance, message) {
  alert(`[${date.getHours()}:${date.getMinutes()}] [${importance}] ${message}`);
}
```

```js
log = _.curry(log);
```

```js
// 柯里化之后，log 仍正常运行：
log(new Date(), 'DEBUG', 'some debug'); // log(a, b, c)
```

```js
// 也可以以柯里化形式运行：
log(new Date())('DEBUG')('some debug'); // log(a)(b)(c)
```

```js
// logNow 会是带有固定第一个参数的日志的偏函数
let logNow = log(new Date());

// 使用它
logNow('INFO', 'message'); // [HH:mm] INFO message
```

#### Summary

柯里化 是一种转换，将 `f(a,b,c)` 转换为可以被以 `f(a)(b)(c)` 的形式进行调用。JavaScript 实现通常都保持该函数可以被正常调用，并且如果参数数量不足，则返回偏函数。

柯里化让我们能够更容易地获取偏函数。就像我们在日志记录示例中看到的那样，普通函数 log(date, importance, message) 在被柯里化之后，当我们调用它的时候传入一个参数（如 log(date)）或两个参数（log(date, importance)）时，它会返回偏函数。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[柯里化（Currying）](https://zh.javascript.info/currying-partials)
