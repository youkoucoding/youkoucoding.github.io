---
title: '毎日のフロントエンド 264~270'
date: 2022-05-18T23:05:00+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### `MIME`

- 媒体类型（通常称为 `Multipurpose Internet Mail Extensions` 或 MIME 类型 ）是一种标准，用来表示*文档、文件或字节流的性质和格式*
- 常用类型
  - `text/plain`
  - `text/html`
  - `image/jpeg`
  - `image/png`
  - `audio/mpeg`
  - `audio/ogg`
  - `audio/*`
  - `video/mp4`
  - `application/*`
  - `application/json`
  - `application/javascript`
  - `application/ecmascript`
  - `application/octet-stream`

### AsyncAwait 优越之处

- Async/Await 的优点：

  - 语法简洁清晰，节省了很多不必要的匿名函数
  - 直接使用 try...catch... 进行异常处理
  - 添加条件判断更符合直觉
  - 减少不必要的中间变量
  - 更清晰明确的错误堆栈
  - 调试时可以轻松给每个异步调用加断点

- Async/Await 是如何实现的
  - 根据 Async/Await 的规范 中的描述 —— 一个 Async 函数总是会返回一个 Promise

```js
async function test() {
  const img = await fetch('tiger.jpg');
}
```

**Babel**

```js
'use strict';

var test = (function () {
  var _ref = _asyncToGenerator(
    regeneratorRuntime.mark(function _callee() {
      var img;
      return regeneratorRuntime.wrap(
        function _callee$(_context) {
          while (1) {
            switch ((_context.prev = _context.next)) {
              case 0:
                _context.next = 2;
                return fetch('tiger.jpg');

              case 2:
                img = _context.sent;

              case 3:
              case 'end':
                return _context.stop();
            }
          }
        },
        _callee,
        this
      );
    })
  );

  return function test() {
    return _ref.apply(this, arguments);
  };
})();

function _asyncToGenerator(fn) {
  return function () {
    var gen = fn.apply(this, arguments);
    return new Promise(function (resolve, reject) {
      function step(key, arg) {
        try {
          var info = gen[key](arg);
          var value = info.value;
        } catch (error) {
          reject(error);
          return;
        }
        if (info.done) {
          resolve(value);
        } else {
          return Promise.resolve(value).then(
            function (value) {
              step('next', value);
            },
            function (err) {
              step('throw', err);
            }
          );
        }
      }
      return step('next');
    });
  };
}
```

- 要想实现真正的异步，还是需要依赖 Promise.all 封装一层：

```js
async function mount() {
  const result = await Promise.all([fetch('a.json'), fetch('b.json'), fetch('c.json')]);

  render(...result);
}
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [MIME 类型 - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

- [6 Reasons Why JavaScript Async/Await Blows Promises Away (Tutorial) | HackerNoon](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9)

- [weekly/4.精读《AsyncAwait 优越之处》.md at master · ascoders/weekly](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/4.%E7%B2%BE%E8%AF%BB%E3%80%8AAsyncAwait%20%E4%BC%98%E8%B6%8A%E4%B9%8B%E5%A4%84%E3%80%8B.md)
