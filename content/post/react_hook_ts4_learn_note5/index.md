---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（五）
date: 2021-09-12T17:48:16+09:00
description: 知识点： 防抖与节流 debounce and throttle
image: throttle-cover.jpg
categories:
  - React
  - TypeScript
---

# 防抖 与 节流

# Debouncing and Throttling in Javascript

> Debouncing and Throttling are two widely-used techniques to improve the performance of code that gets executed repeatedly within a short time(a peroid of time).

![devbounce-throttle](debounce-throttle.png)

## Debouncing 防抖

用来实现高频触发函数调用时，实际只调用最后一次函数执行。

即： 触发事件后过一段时间才执行函数， 如果在这段时间内再次触发，则重新计时。

用于可能出现高频调用的 DOM 或 样式修改， 这种高频调用会导致页面高频重排重绘，DOM 闪烁抖动，影响页面性能。

#### Implementing Debounce:

1. Start with o timeout
2. If the debounced function is called again, reset the timer to the speccial delay
3. In case of timeout, call the debounced function.

Thus every call to a debounce function resets the timer and delays the call.

Debounce is higher-order function, which is a function that returns anotehr function. This is done to form a closure around the `handler` and `delay` function parameters and the `timer` variable so that their values are preserved.

**If we are invoking for the first time, our function will execute at the end of our delay**. If we invoke and then invoke again before the end of our delay, the delay restarts.

> 调用 `setTimeout` 会返回一个 `timeoutId` , 然后通过调用 `clearTimeout` 清除 制定的 `timeoutId`

#### `self` 和 `apply`

- 在 `apply` 中 会将调用的函数（`handler`）， 绑定到 `this` 中， 所以如果 不用 之前创建的 `self` 而是直接使用 `this`, 可能会产生问题，比如： 需要防抖的`handler`是在全局环境中调用时， `this` 执行上下文是 `window`， 不能指向绑定事件的元素上。 因此，使用 `apply` 显式地 将当前的执行上下文的`this` 绑定到 `handler` 上，以使元素上的`handler`生效。

```js
//  Debounce
function debounce(handler, delay) {
  let timer = null;
  return function () {
    let self = this;
    let args = arguments; // arguments 是一个类数组对象， 指向传入的参数。
    if (timer) clearTimeout(timer); // 清除前一个需要debounce 的函数
    timer = setTimeout(function () {
      // apply 会将调用的函数, 绑定到 apply第一个参数指向的上下文中(此处的 self)， 并且可以接受一个数组或类数组对象作为后续参数
      handler.apply(self, args);
    }, delay);
  };
}

//test
function testDebounce() {
  console.log('this is a test.');
}
document.onmousemove = function () {
  debounce(testDebounce);
};
```

#### 用箭头函数简化

- 箭头函数的 `this` 指向函数定义时 上下文`this`
- 使用扩展运算符，避免定义 `arguments`

```js
function debounce(handler, delay = 1000) {
  let timer = null;
  return (...args) => {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      handler(args);
    }, delay);
  };
}
```

---

## Throttling 节流

Throttling or sometimes is also called throttle function is a practise used in websites. To throttle a function means to ensure that the function is called at most once in a specified time period. This means throttling will prevent a function from running if it has run "recently". Throttling also ensures a function is run regularly at a fixed rate.

Throttlig is used to call a function after every millisecond or a particular interval of time only the first click is executed immediately.

用来实现**_阻止_**在短时间内重复多次触发同一个函数。

即： 每一个时间间隔内， 只执行一次函数, `timer` 存在的时候 直接返回，不存在的时候，执行`setTimeout`(执行完，会清空`timer`)

#### Implementing Throttle:

```js
// Throttle
function throttle(handler, delay) {
  let timer = null;
  return function () {
    let self = this,
      args = arguments;

    if (timer) return;

    timer = setTimeout(function () {
      handler.apply(self, args);
      timer = null;
    }, delay);
  };
}

// test
function testThrottle(e, content) {
  console.log(e, content);
}

var testThrottleHandler = throttle(testThrottle, 1000);

document.onmousemove = function (e) {
  testThrottleHandler(e, 'throttle');
};
```

#### 使用 箭头函数 优化

```js
function throttle(handler, delay) {
  let timer = null;

  return (...args) => {
    if (timer) return;
    timer = setTimeout(() => {
      handler(args);
      timer = null;
    }, delay);
  };
}
```

```js
// example in reference
const throttle = (func, limit) => {
  let lastFunc;
  let lastRan;
  return function () {
    const context = this;
    const args = arguments;
    if (!lastRan) {
      func.apply(context, args);
      lastRan = Date.now();
    } else {
      clearTimeout(lastFunc);
      lastFunc = setTimeout(function () {
        if (Date.now() - lastRan >= limit) {
          func.apply(context, args);
          lastRan = Date.now();
        }
      }, limit - (Date.now() - lastRan));
    }
  };
};
```

The first call to the function will execute and sets the limit period `delay`. We can call the function during this period but it will not fire until the `throttle` period has passed. Once is has passed, the next invocation will fire and the process repeats.

## Compare Debounce and Throttle

### Similarities

1. 都使用了 `setTimeout`

2. 目的都是降低回调函数的执行频率， 节省资源。

### Difference

1. `Debounce` 防抖关注的是：一定时间段内，连续触发的事件，只在最后一次触发的时候 执行。

2. `Throttle` 节流关注的是：侧重一个时间间隔内，只执行一次。

### 使用场景 `use cases`

1. `Debounce` **防抖的使用场景：** 连续的事件，只需要触发一次的场景， 例如：

   - 搜索框 输入搜索内容，最后输入完成后，再发送请求；
   - 号码， 邮箱的输入验证；
   - 窗口大小的调整： 在调整完成后再计算窗口大小，防止重复渲染。
   - _Autocomplete_: Often times, search boxes offer dropdowns that provide autocomplete options for the user's current input. Sometimes the items suggested are fetched from the backend API. Here, debouncing can be applied in implementing suggestive text where we wait for the user to stop typing for a few seconds before suggesting the text. Thus, on every keystrokem, we wait for some seconds before giving out suggestions.
   - Debouncing a `resize` event handler.
   - Debouncing a save function in an autosave feature.
   - Don't do anything whiule the user drags and drops.
   - Don't make any Axios requests until the user stops typing.

2. `Throttle` **节流的使用场景：** 间隔一段时间之行一次回调函数的场景：

   - 滚动加载 或者 加载更多的场景
   - 表单的多次点击提交
   - _Gaming_
   - _Scroll event handler_
   - Throttling a button cliock so er can't spam click
   - Throttling an API call
   - Throttling a `mousemove/touchmove` event handler.

## Reference

[javaScript 节流与防抖](https://www.huaweicloud.com/articles/289941d93ba2f7dc048d790d1697f80d.html)

[JavaScript 防抖与节流 - YouTube](https://www.youtube.com/watch?v=KviNB_deCaI&t=202s)

[Debouncing and Throttling in JavaScript: Comprehensive Guide | by Ayush Verma | Towards Dev](https://towardsdev.com/debouncing-and-throttling-in-javascript-8862efe2b563)
