---
title: '毎日のフロントエンド　14'
date: 2021-09-28T15:34:08+09:00
description: frontend 每日一练
image: frontend-14-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十四日

## HTML

### **#Question:** 为什么 HTML5 只需要写 `<!DOCTYPE HTML>` 就可以？

> The html document type declaration, also known as `DOCTYPE`, is the first line of code required in every HTML or xHTML document. The `DOCTYPE` declaration us an instruction to the web browser about what version of HTML the page is written in. This ensures that the web page is parsed the same way by different web browsers.

The declaration of `HTML5` `DOCTYPE` is much simpler: it no longer requires a reference to **DTDs** as it is **no longer based on `SGML`**.

Doctype syntax for HTML5 and beyond:

```html
<!DOCTYPE html>
```

HTML5 与 HTML4 基于的基准不同。HTML4 基于 `SGML` 因此需要除了 DOCTYPE 外还需要引入 `DTD` 来告诉浏览器用什么标准进行渲染。DTD 还分为 _标准模式、严格模式。_

HTML5 不基于 SGML，因此后面就不要跟 DTD，但是需要 DOCTYPE 来规范浏览器的渲染行为。

> `SGML` 是通用标记语言的集合。其中有 HTML、XML，因此需要用 DTD 来指定使用那种规范。

## CSS

### **#Question:** `position:fixed;` 在 ios 下无效该怎么办？

移动端业务开发，iOS 下经常会有 fixed 元素和输入框(input 元素)同时存在的情况。 但是 fixed 元素在有软键盘唤起的情况下，会出现许多莫名其妙的问题。

现象： 当采用 fixed 做吸底、吸顶布局时，如果触发键盘弹出事件则 fixed 属性会失效，布局就会被扰乱。其原因解释如下：

软键盘唤起后，页面的 fixed 元素将失效（即无法浮动，也可以理解为变成了 absolute 定位），所以当页面超过一屏且滚动时，失效的 fixed 元素就会跟随滚动了。

- 第三方库 **`isScroll.js`** 可以解决此问题。

## JavaScript

### **#Question:** 什么是闭包？优缺点分别是什么？ What is a Closure?

> Closures is are frequently **used in JavaScript for object data privacy**, in evently handlers and callback functions, and in partial application, currying(柯里化)，and other functional programming patterns.

#### What is Closure?

A closure is the combination of a function bundled together with references to **its surrounding state** (the **lexical environment**). In other words, **a closure gives you access to an outer function's scope from an inner one.**

IN JavaScript, closures are created every time a function is created, at function creation time.

To use a closure, define a function inside another function and expose it. To expose a function, return it or pass it to another function.

The inner function will have access to the variables in the outer one's scope, even after the outer function has returned.

#### Examples

Data privacy is an essential property that helps us **program to an interface, not an implementation.** And among other things, closures are commonly used to give objects data privacy.

This is an important concept that helps us build more robust software because implementation details are more likely to change in breaking ways than interface contracts.

In javascript, closures are the primary mechanism used to enable data privacy. When you use closures for data privacy, the enclosed varibles are only in scope within the containing (outer) function. You can't get at the data from an outside scope except through the object's provileged methods.

闭包是可以访问另一个函数作用域的函数。由于 javascript 的特性，外层的函数无法访问内部函数的变量；而内部函数可以访问外部函数的变量（即作用域链）。

```js
function a() {
  var b = 1;
  var c = 2;
  // 这个函数就是个闭包，可以访问外层 a 函数的变量
  return function () {
    var d = 3;
    return b + c + d;
  };
}

var e = a();
console.log(e());
```

- 使用闭包可以**隐藏变量**以及**防止变量被篡改和作用域的污染**，从而实现封装

- 缺点就是由于保留了作用域链，会增加内存的开销。因此需要注意内存的使用，并且防止内存泄露的问题。

## Reference

[What is the DOCTYPE Declaration in HTML?](https://www.freecodecamp.org/news/what-is-the-doctype-declaration-in-html/)

[Web 移动端 Fixed 布局的解决方案 | EFE Tech](https://efe.baidu.com/blog/mobile-fixed-layout/)

[关于闭包 - CNode 技术社区](https://cnodejs.org/topic/5d39c5259969a529571d73a8)
