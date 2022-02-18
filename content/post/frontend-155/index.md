---
title: '毎日のフロントエンド  155'
date: 2022-02-18T10:51:40+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
  - Tips
---

# 第一百五十五日

## HTML

### HTML5 有哪些存储类型

- `cookie`: 最大 4k, 基本无兼容问题, 所有同源 tab 共享, 每次请求都携带, key-value 存储, value 只存字符串
- `sessionStorage` 无大小限制, 只在当前 tab 有效, tab 关闭即失效, key-value 存储, value 只存字符串
- `localStorage` 最大 5M-10M, 所有同源 tab 共享, 能持久化存储, key-value 存储, value 只存字符串
- `indexDB` key-value 存储,value 可以任意类型, 同源, 支持事务, 最大 250M, 兼容 ie10
- `webSQL` 支持版本,事务,支持 sql 语句, 不兼容 ie

## CSS

[CSS Guidelines (2.2.5) – High-level advice and guidelines for writing sane, manageable, scalable CSS](https://cssguidelin.es/#the-importance-of-a-styleguide)

## JavaScript

### 实现一个`Promise`

- `Promise/A+` 规范

  1. `promise`：是一个具有 then 方法的对象或者函数，它的行为符合该规范。
  2. `thenable`：是一个定义了 then 方法的对象或者函数。
  3. `value`：可以是任何一个合法的 JavaScript 的值（包括 undefined、thenable 或 promise）。
  4. `exception`：是一个异常，是在 Promise 里面可以用 throw 语句抛出来的值。
  5. `reason`：是一个 Promise 里 reject 之后返回的拒绝原因。

- `Promise` 状态描述
  1. 一个 Promise 有三种状态：`pending`、`fulfilled` 和 `rejected`。
  2. 当状态为 pending 状态时，即可以转换为 fulfilled 或者 rejected 其中之一。
  3. 当状态为 fulfilled 状态时，就不能转换为其他状态了，必须返回一个不能再改变的值。
  4. 当状态为 rejected 状态时，同样也不能转换为其他状态，必须有一个原因的值也不能改变。

#### `then` 方法

- `Promise` 必须拥有一个 `then` 方法来**访问它的值或者拒绝原因**

  - `promise.then(onFulfilled, onRejected)`
  - onFulfilled 和 onRejected 都是可选参数

- `onFulfilled` 和 `onRejected` 特性

  - 如果 onFulfilled 是函数，则当 Promise 执行结束之后必须被调用，最终返回值为 value，其调用次数不可超过一次
  - onRejected 除了最后返回的是 reason 外，其他方面和 onFulfilled 在规范上的表述基本一样

- then 方法其实可以被一个 Promise 调用多次，且必须返回一个 Promise 对象

#### 第一步：构造函数

- Promise 构造函数接受一个 executor 函数，executor 函数执行完同步或者异步操作后，调用它的两个参数 resolve 和 reject

```js
function Promise(executor) {
  var self = this;
  self.status = 'pending'; // Promise当前的状态
  self.data = undefined; // Promise的值
  self.onResolvedCallback = []; // Promise resolve时的回调函数集
  self.onRejectedCallback = []; // Promise reject时的回调函数集
  executor(resolve, reject); // 执行executor并传入相应的参数
}
```

#### 第二步： 完善 `resolve` 和 `reject` 两个函数

```js
function Promise(executor) {
  var self = this;
  self.status = 'pending'; // Promise当前的状态
  self.data = undefined; // Promise的值
  self.onResolvedCallback = []; // Promise resolve时的回调函数集
  self.onRejectedCallback = []; // Promise reject时的回调函数集

  // 由于 onResolveCallback 和 onRejectedCallback 这两个是数组，需要通过循环来执行
  function resolve(value) {
    if (self.status === 'pending') {
      self.status = 'resolved';
      self.data = value;
      for (var i = 0; i < self.onResolvedCallback.length; i++) {
        self.onResolvedCallback[i](value);
      }
    }
  }
  function reject(reason) {
    if (self.status === 'pending') {
      self.status = 'rejected';
      self.data = reason;
      for (var i = 0; i < self.onRejectedCallback.length; i++) {
        self.onRejectedCallbacki;
      }
    }
  }

  try {
    // 考虑到执行过程中有可能出错，所以我们用try/catch块给包起
    executor(resolve, reject); // 执行executor
  } catch (e) {
    reject(e);
  }
}
```

#### 第三步： 实现 `then` 方法

- then 方法是 Promise 执行完之后可以拿到 value 或者 reason 的方法，并且还要保持 then 执行之后，返回的依旧是一个 Promise 方法，还要支持多次调用

```js
// then方法接收两个参数onResolved和onRejected，分别为Promise成功或失败后的回调
Promise.prototype.then = function (onResolved, onRejected) {
  var self = this;
  var promise2;
  // 根据标准，如果then的参数不是function，则需要忽略它
  onResolved = typeof onResolved === 'function' ? onResolved : function (value) {};
  onRejected = typeof onRejected === 'function' ? onRejected : function (reason) {};
  if (self.status === 'resolved') {
    // 如果promise1的状态已经确定并且是resolved
    // 调用onResolved，考虑到有可能throw，所以还需要将其包在try/catch块里
    return (promise2 = new Promise(function (resolve, reject) {
      try {
        var x = onResolved(self.data);
        if (x instanceof Promise) {
          // 如果onResolved的返回值是一个Promise对象，直接取它的结果作为promise2的结果
          x.then(resolve, reject);
        }
        resolve(x); // 否则，以它的返回值作为promise2的结果
      } catch (e) {
        reject(e); // 如果出错，以捕获到的错误作为promise2的结果
      }
    }));
  }
  // 此处与前一个if块的逻辑几乎相同，区别在于所调用的是onRejected函数
  if (self.status === 'rejected') {
    return (promise2 = new Promise(function (resolve, reject) {
      try {
        var x = onRejected(self.data);
        if (x instanceof Promise) {
          x.then(resolve, reject);
        }
      } catch (e) {
        reject(e);
      }
    }));
  }
  if (self.status === 'pending') {
    // 如果当前的Promise还处于pending状态，
    // 并不能确定调用onResolved还是onRejected，只能等到Promise的状态确定后，才能确定如何处理
    return (promise2 = new Promise(function (resolve, reject) {
      self.onResolvedCallback.push(function (value) {
        try {
          var x = onResolved(self.data);
          if (x instanceof Promise) {
            x.then(resolve, reject);
          }
        } catch (e) {
          reject(e);
        }
      });
      self.onRejectedCallback.push(function (reason) {
        try {
          var x = onRejected(self.data);
          if (x instanceof Promise) {
            x.then(resolve, reject);
          }
        } catch (e) {
          reject(e);
        }
      });
    }));
  }
};
```

- 在 Promise/A+ 规范中，onResolved 和 onRejected 这两项函数需要异步调用

#### 完成版

```js
try {
  module.exports = Promise;
} catch (e) {}

function Promise(executor) {
  var self = this;
  self.status = 'pending';
  self.onResolvedCallback = [];
  self.onRejectedCallback = [];
  function resolve(value) {
    if (value instanceof Promise) {
      return value.then(resolve, reject);
    }
    setTimeout(function () {
      // 异步执行所有的回调函数
      if (self.status === 'pending') {
        self.status = 'resolved';
        self.data = value;
        for (var i = 0; i < self.onResolvedCallback.length; i++) {
          self.onResolvedCallback[i](value);
        }
      }
    });
  }
  function reject(reason) {
    setTimeout(function () {
      // 异步执行所有的回调函数
      if (self.status === 'pending') {
        self.status = 'rejected';
        self.data = reason;
        for (var i = 0; i < self.onRejectedCallback.length; i++) {
          self.onRejectedCallback[i](reason);
        }
      }
    });
  }
  try {
    executor(resolve, reject);
  } catch (reason) {
    reject(reason);
  }
}
function resolvePromise(promise2, x, resolve, reject) {
  var then;
  var thenCalledOrThrow = false;
  if (promise2 === x) {
    return reject(new TypeError('Chaining cycle detected for promise!'));
  }
  if (x instanceof Promise) {
    if (x.status === 'pending') {
      x.then(function (v) {
        resolvePromise(promise2, v, resolve, reject);
      }, reject);
    } else {
      x.then(resolve, reject);
    }
    return;
  }
  if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
    try {
      then = x.then;
      if (typeof then === 'function') {
        then.call(
          x,
          function rs(y) {
            if (thenCalledOrThrow) return;
            thenCalledOrThrow = true;
            return resolvePromise(promise2, y, resolve, reject);
          },
          function rj(r) {
            if (thenCalledOrThrow) return;
            thenCalledOrThrow = true;
            return reject(r);
          }
        );
      } else {
        resolve(x);
      }
    } catch (e) {
      if (thenCalledOrThrow) return;
      thenCalledOrThrow = true;
      return reject(e);
    }
  } else {
    resolve(x);
  }
}
Promise.prototype.then = function (onResolved, onRejected) {
  var self = this;
  var promise2;
  onResolved =
    typeof onResolved === 'function'
      ? onResolved
      : function (v) {
          return v;
        };
  onRejected =
    typeof onRejected === 'function'
      ? onRejected
      : function (r) {
          throw r;
        };
  if (self.status === 'resolved') {
    return (promise2 = new Promise(function (resolve, reject) {
      setTimeout(function () {
        // 异步执行onResolved
        try {
          var x = onResolved(self.data);
          resolvePromise(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }
  if (self.status === 'rejected') {
    return (promise2 = new Promise(function (resolve, reject) {
      setTimeout(function () {
        // 异步执行onRejected
        try {
          var x = onRejected(self.data);
          resolvePromise(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }
  if (self.status === 'pending') {
    // 这里之所以没有异步执行，是因为这些函数必然会被resolve或reject调用，
    // 而resolve或reject函数里的内容已是异步执行，构造函数里的定义
    return (promise2 = new Promise(function (resolve, reject) {
      self.onResolvedCallback.push(function (value) {
        try {
          var x = onResolved(value);
          resolvePromise(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });
      self.onRejectedCallback.push(function (reason) {
        try {
          var x = onRejected(reason);
          resolvePromise(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });
    }));
  }
};
Promise.prototype.catch = function (onRejected) {
  return this.then(null, onRejected);
};
// 最后这个是测试用的，后面会说
Promise.deferred = Promise.defer = function () {
  var dfd = {};
  dfd.promise = new Promise(function (resolve, reject) {
    dfd.resolve = resolve;
    dfd.reject = reject;
  });
  return dfd;
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[微信红包算法(js)](https://www.jianshu.com/p/11494478a7e9)

[promises-aplus/promises-tests: Compliances tests for Promises/A+](https://github.com/promises-aplus/promises-tests)
