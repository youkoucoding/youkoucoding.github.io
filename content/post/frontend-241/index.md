---
title: '毎日のフロントエンド 241'
date: 2022-05-09T23:33:18+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `<a>`除了用作跳转链接外，还有哪些用途

- [`<a>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)

## CSS

### `vmax` 和 `vmin`

- `vmax`和`vmin`都是相对于窗口大小的长度单位。
- `100vmax`相当于 100%当前窗口长或者宽的长度，取其中最大值，vmin 反之。
- 如果 css 函数`max()`和`min()`普及后，`100vmax` 相当于 `max(100vw, 100vh)`
- 如`vmin`，可以在确保在保持宽高比的情况下，不论窗口如何缩放都可以不让元素超出窗口范围

## Tips

### `try..catch` 不能捕获的错误有哪些

1. `try`

   - 每个`try`块必须与至少一个`catch`或`finally`块，否则会抛出`SyntaxError`错误

2. `try..catch` 与 无效代码
   - try..catch 无法捕获无效的 JS 代码，例如 try 块中的以下代码在语法上是错误的，但它不会被 catch 块捕获

```js
try {
  ~!$%^&*
} catch(err) {
  console.log("不会执行");
}
```

3. `try..catch` 与 异步代码
   - `try..catch` 无法捕获在异步代码中引发的异常，例如 `setTimeout`：

```js
try {
  setTimeout(function () {
    noSuchVariable; // undefined variable
  }, 1000);
} catch (err) {
  console.log('不会执行');
}

// 未捕获的ReferenceError将在1秒后引发:
// Uncaught ReferenceError: noSuchVariable is not defined
```

**应该在异步代码内部使用 try..catch 来处理错误：**

```js
setTimeout(function () {
  try {
    noSuchVariable;
  } catch (err) {
    console.log('error is caught here!');
  }
}, 1000);
```

4. 嵌套 `try..catch`
   - 可以使用嵌套的 try 和 catch 块向上抛出错误

```js
try {
  try {
    throw new Error('Error while executing the inner code');
  } catch (err) {
    throw err;
  }
} catch (err) {
  console.log('Error caught by outer block:');
  console.error(err.message);
}

// Error caught by outer block:
// ➤ ⓧ Error while executing the code
```

5. `try..finally`
   - 不建议仅使用 try..finally 而没有 catch 块

```js
try {
  throw new Error('Error while executing the code');
} finally {
  console.log('finally');
}

// finally
//➤ ⓧ Uncaught Error: Error while executing the code
```

- 即使从 try 块抛出错误后，也会执行 finally 块
- 如果没有 catch 块，错误将不能被优雅地处理，从而导致未捕获的错误

6. `try..catch..finally`
   - 建议使用 `try...catch` 块和可选的 `finally` 块

```js
try {
  console.log('Start of try block');
  throw new Error('Error while executing the code');
  console.log('End of try block -- never reached');
} catch (err) {
  console.error(err.message);
} finally {
  console.log('Finally block always run');
}
console.log('Code execution outside try-catch-finally block continue..');

// Start of try block
// ➤ ⓧ Error while executing the code
// Finally block always run
// Code execution outside try-catch-finally block continue..
```

- 在 try 块中抛出错误后往后的代码不会被执行了
- 即使在 try 块抛出错误之后，finally 块仍然执行

- finally 块通常用于清理资源或关闭流，如下所示：

```js
try {
  openFile(file);
  readFile(file);
} catch (err) {
  console.error(err.message);
} finally {
  closeFile(file);
}
```

6. `throw`

```js
throw <expression>

// throw primitives and functions
throw "Error404";
throw 42;
throw true;
throw {toString: function() { return "I'm an object!"; } };

// throw error object
throw new Error('Error while executing the code');
throw new SyntaxError('Something is wrong with the syntax');
throw new ReferenceError('Oops..Wrong reference');

// throw custom error object
function ValidationError(message) {
  this.message = message;
  this.name = 'ValidationError';
}
throw new ValidationError('Value too high');
```

#### 异步代码中的错误处理

1. Promise 中的 `then..catch`
   - 使用 `then()`和 `catch()`链接多个 Promises，以处理链中单个 Promise 的错误

```js
Promise.resolve(1)
  .then((res) => {
    console.log(res); // 打印 '1'

    throw new Error('something went wrong'); // throw error

    return Promise.resolve(2); // 这里不会被执行
  })
  .then((res) => {
    // 这里也不会执行，因为错误还没有被处理
    console.log(res);
  })
  .catch((err) => {
    console.error(err.message); // 打印 'something went wrong'
    return Promise.resolve(3);
  })
  .then((res) => {
    console.log(res); // 打印 '3'
  })
  .catch((err) => {
    // 这里不会被执行
    console.error(err);
  });
```

- 使用 fetch 调用 API，该 API 返回一个 promise 对象，我们使用 catch 块优雅地处理 API 失败

```js
function handleErrors(response) {
  if (!response.ok) {
    throw Error(response.statusText);
  }
  return response;
}

fetch('http://httpstat.us/500')
  .then(handleErrors)
  .then((response) => console.log('ok'))
  .catch((error) => console.log('Caught', error));

//  Caught Error: Internal Server Error
//    at handleErrors
```

2. try..catch 和 async await
   - 在 async await 中 使用 try..catch

```js
(async function () {
  try {
    await fetch('http://httpstat.us/500');
  } catch (err) {
    console.error(err.message);
  }
})();
```

- 使用 fetch 调用 API，该 API 返回一个 promise 对象， 使用 try..catch 块优雅地处理 API 失败

```js
function handleErrors(response) {
  if (!response.ok) {
    throw Error(response.statusText);
  }
}

(async function () {
  try {
    let response = await fetch('http://httpstat.us/500');
    handleErrors(response);
    let data = await response.json();
    return data;
  } catch (error) {
    console.log('Caught', error);
  }
})();

// Caught Error: Internal Server Error
//     at handleErrors (<anonymous>:3:15)
//     at <anonymous>:11:7
```

#### JS 中的内置错误

- JavaScript 有内置的错误对象，它通常由 try 块抛出，并在 catch 块中捕获，Error 对象包含以下属性：
  - name：是错误的名称，例如 “Error”, “SyntaxError”, “ReferenceError” 等。
  - message：有关错误详细信息的消息。
  - stack：是用于调试目的的错误的堆栈跟踪。

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [CSS values and units - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Values_and_Units)
