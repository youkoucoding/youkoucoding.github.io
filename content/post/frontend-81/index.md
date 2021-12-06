---
title: '毎日のフロントエンド　 81'
date: 2021-12-06T10:51:02+09:00
description: frontend 每日一练
image: frontend-81-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十一日

## HTML

### **Question:** `HTML5`的`Device API`应用场景

- dial: 拨打电话
- vibrate: 设备振动
- setWakelock: 设置应用是否保持唤醒（屏幕常亮）状态
- isWakelock: 获取程序是否一直保持唤醒（屏幕常亮）状态
- setVolume: 设置设备的系统音量
- getVolume: 获取设备的系统音量

## JavaScript

### **Question:** 在 js 中怎么捕获异常，应用场景

Js 捕获异常的方法:

- `try catch finally`
- `window.onerror = function(message, source, lineno, colno, error) { ... }`

#### `try catch finally`

`try catch finally` 只能捕获**运行时**的错误，无法捕获语法错误，可以拿到出错的信息，堆栈，出错的文件、行号、列号。`try catch finally` 语句标记要尝试的语句块，并指定一个出现异常时抛出的响应。

```js
try {
  // try_statements
  throw new TypeError('Test');
} catch (e) {
  // catch_statements
  console.log('catch_statements');
  if (e instanceof TypeError) {
    // handle this expected error
    console.log('handle this expected error');
  } else {
    // handle unexpected error
    console.log('handle unexpected error');
  }
} finally {
  // finally_statements
  console.log('finally_statements');
}

/*
    注：
    [catch (e if e instanceof TypeError) { // 非标准
       catch_statements
    }]
*/
```

通过`Error`的构造器可以创建一个错误对象，当运行时错误产生时，`Error`的实例对象会被抛出，`Error`对象也可用于用户自定义的异常的基础对象，Js 内建了几种标准错误类型：

- `EvalError:` 创建一个`error`实例，表示错误的原因：与 eval()有关。
- `RangeError:` 创建一个`error`实例，表示错误的原因：数值变量或参数超出其有效范围。
- `ReferenceError:` 创建一个`error`实例，表示错误的原因：无效引用。
- `SyntaxError:` 创建一个`error`实例，表示错误的原因：eval()在解析代码的过程中发生的语法错误。
- `TypeError:` 创建一个`error`实例，表示错误的原因：变量或参数不属于有效类型。
- `URIError:` 创建一个`error`实例，表示错误的原因：给 encodeURI()或 decodeURl()传递的参数无效。

#### `window.onerror`

`window.onerror`可以**捕捉语法错误，也可以捕捉运行时错误**，可以拿到出错的信息，堆栈，出错的文件、行号、列号，只要在当前`window`执行的 Js 脚本出错都会捕捉到，通过`window.onerror`可以实现前端的错误监控。出于安全方面的考虑，当加载自不同域的脚本中发生语法错误时，语法错误的细节将不会报告。

```js
/*
    message：错误信息（字符串）。
    source：发生错误的脚本URL（字符串）
    lineno：发生错误的行号（数字）
    colno：发生错误的列号（数字）
    error：Error对象（对象）
    若该函数返回true，则阻止执行默认事件处理函数。
*/
window.onerror = function (message, source, lineno, colno, error) {
  // onerror_statements
};

/*
    ErrorEvent类型的event包含有关事件和错误的所有信息。
*/
window.addEventListener('error', function (event) {
  // onerror_statements
});
```

### **Question:** 下面代码中 a 在什么情况下会打印 1

```js
var a = ?;
if(a == 1 && a== 2 && a== 3){
 	console.log(1);
}
```

> 实际情况中，应规定禁用 `==` 运算符换用`===` 严格相等。

Solution 1:

```js
var a = {
  i: 1,
  toString: function () {
    return a.i++;
  },
};
if (a == 1 && a == 2 && a == 3) {
  console.log('1');
}
```

- 如果原始类型的值和对象比较，对象会转为原始类型的值，再进行比较。
- 对象转换成原始类型的值，算法是先调用 `valueOf()`；如果返回的还是对象，再接着调用 `toString()`。

Solution 2:

```js
var a = [1, 2, 3];
a.join = a.shift;
console.log(a == 1 && a == 2 && a == 3);
```

- `array` 也属于对象
- 对于数组对象，`toString()` 方法**返回一个字符串**，该字符串由数组中的每个元素的 `toString()` 返回值经调用 `join()` 方法连接（由逗号隔开）组成
- 数组 `toString` 会调用本身的 `join` 方法，这里把自己的 `join` 方法该写为 `shift`,每次返回第一个元素，而且原数组删除第一个值，正好可以使判断成立

Solution 3:

```js
var val = 0;
Object.defineProperty(window, 'a', {
  get: function () {
    return ++val;
  },
});
if (a == 1 && a == 2 && a == 3) {
  console.log('1');
}
```

- 全局变量也相当于 `window` 对象上的一个属性，这里用`defineProperty` 定义了 `a` 的 `get` 也使得其动态返回值。和`with`有一些类似。

Solution 4:

```js
let a = {
  [Symbol.toPrimitive]: (
    (i) => () =>
      ++i
  )(0),
};
if (a == 1 && a == 2 && a == 3) {
  console.log('1');
}
```

- ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。
  - 定义类的内部私有属性时候习惯用 `__x` ,这种命名方式避免别人定义相同的属性名覆盖原来的属性，完全可以用 `Symbol` 值来代替这种方法，而且完全不用担心被覆盖。
- ES6 还提供了 11 个内置的 `Symbol` 值，指向语言内部使用的方法。
  - `Symbol.toPrimitive` 就是其中一个，它指向一个方法，表示该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。这里就是改变这个属性，把它的值改为一个 **Instantly Invoked Function Expression 即时调用函数表达式** 返回的函数。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[GlobalEventHandlers.onerror - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onerror)
