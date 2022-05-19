---
title: '毎日のフロントエンド 271~275'
date: 2022-05-19T21:06:57+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### 移动端禁止用户手动缩放页面

- `<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no">`

### `Array.prototype.sort()`

- `sort()` 方法用原地算法对数组的元素进行排序，并返回数组。
  - 默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的

### JS 错误堆栈处理

> JavaScript 中主要是通过 `Error` 对象和 `Stack Traces` 提供有价值的错误堆栈，帮助开发者调试。在服务端开发中，开发者可以将有价值错误信息打印到服务器日志中，而对于客户端而言就很难重现用户环境下的报错

#### **Stack**

- js 中函数调用栈的概念，符合栈的基本特性『当调用时，压入栈顶。当它执行完毕时，被弹出栈』

```js
function c() {
  try {
    var bar = baz;
    throw new Error();
  } catch (e) {
    console.log(e.stack);
  }
}

function b() {
  c();
}

function a() {
  b();
}

a();
```

- 上述代码中会在执行到 c 函数的时候跑错，调用栈为 `a -> b -> c`, 错误堆栈可以帮助定位到报错的位置

#### **Error 对象**

- Error 对象，主要有两个重要属性 message 和 name 分别表示*错误信息*和*错误名称*, 除了这两个属性还有一个未被标准化的 stack 属性
  - `e.stack`，这个属性包含了错误信息、错误名称以及错误栈信息 [Stack Trace](https://sentry.io/features/stacktrace/)

#### 如何使用堆栈追踪

- 以 NodeJS 环境为例，讲解了 Error.captureStackTrace，将 stack 信息作为属性存储在一个对象当中，同时可以过滤掉一些无用的堆栈信息。这样可以隐藏掉用户不需要了解的内部细节。
- 作者也以 Chai 为例，内部使用该方法对代码的调用者屏蔽了不相关的实现细节。通过以 Assertion 对象为例，讲述了具体的内部实现，简单来说通过一个 addChainableMethod 链式调用工具方法，在运行一个 Assertion 时，将它设为标记，其后面的堆栈会被移除；
  - 如果 assertion 失败移除起后面所有内部堆栈；
  - 如果有内嵌 assertion，将当前 assertion 的方法放到 ssfi 中作为标记，移除后面堆栈帧；

#### 抛 Error 对象的正确方法

- 正确的做法应该是使用 throw new Error(“error message here”)，这里还引用了 [Node.js 中推荐的异常处理方式:](https://www.joyent.com/node-js/production/design/errors)
  - 区分操作异常和程序员的失误。操作异常指可预测的不可避免的异常，如无法连接服务器
  - 操作异常应该被处理。程序员的失误不需要处理，如果处理了反而会影响错误排查
  - 操作异常有两种处理方式：同步 (`try……catch`) 和异步（callback, event - emitter）两种处理方式，_但只能选择其中一种_
  - 函数定义时应该用文档写清楚参数类型，及可能会发生的合理的失败。以及错误是同步还是异步传给调用者的
  - 缺少参数或参数无效是程序员的错误，一旦发生就应该 throw。 传递错误时，使用标准的 Error 对象，并附件尽可能多的错误信息，可以使用标准的属性名

#### 异步（Promise）环境下错误处理方式

- 在 Promise 内部使用 reject 方法来处理错误，而不要直接调用 throw Error，这样你不会捕捉到任何的报错信息
- reject 如果使用 Error 对象，会导致捕获不到错误的情况

```js
function thirdFunction() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject('我可以被捕获');
      // throw Error('永远无法被捕获')
    });
  });
}

Promise.resolve(true)
  .then((resolve, reject) => {
    return thirdFunction();
  })
  .catch((error) => {
    console.log('捕获异常', error); // 捕获异常 我可以被捕获
  });
```

- 在 macrotask 队列中，reject 行为是可以被 catch 到的，而此时 throw Error 就无法捕获异常，第二次把 reject('可以被捕获') 注释起来，取消 throw Error('永远无法被捕获') 的注释，会发现异常无法 catch。
  - 因为 setTimeout 中 throw Error 无论如何都无法捕获到，而 reject 是 Promise 提供的关键字，自己当然可以 catch

#### 监控客户端 Error 报错

- `try...catch` 可以拿到出错的信息，堆栈，出错的文件、行号、列号等，但*无法捕捉到语法错误*，_没法去捕捉全局的异常事件_ 在一些古老的浏览器下 try...catch 对 js 的性能也有一定的影响。
- 另一个捕捉异常的方法，即 `window.onerror` 可以捕捉语法错误和运行时错误，并且拿到出错的信息，堆栈，出错的文件、行号、列号等
  - 由于是全局监测，就会统计到浏览器插件中的 js 异常
  - 给应用内所需的 `<script>` 标签添加 `crossorigin` 属性；
  - 在 js 所在的 cdn 服务器上添加 `Access-Control-Allow-Origin: *` HTTP 头；

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [响应式设计 - 学习 Web 开发 | MDN](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Responsive_Design#%E8%A7%86%E5%8F%A3%E5%85%83%E6%A0%87%E7%AD%BE)

- [Array.prototype.sort() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

- [JavaScript Errors and Stack Traces in Depth](https://lucasfcosta.com/2017/02/17/JavaScript-Errors-and-Stack-Traces.html?utm_source=javascriptweekly&utm_medium=email)

- [weekly/6.精读《JavaScript 错误堆栈处理》.md at master · ascoders/weekly](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/6.%E7%B2%BE%E8%AF%BB%E3%80%8AJavaScript%20%E9%94%99%E8%AF%AF%E5%A0%86%E6%A0%88%E5%A4%84%E7%90%86%E3%80%8B.md)

- [Stack Trace Enhancements and Filters | Sentry](https://sentry.io/features/stacktrace/)

- [Error Handling | Joyent](https://www.joyent.com/node-js/production/design/errors)
