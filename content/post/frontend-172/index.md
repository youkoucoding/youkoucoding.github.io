---
title: '毎日のフロントエンド  172'
date: 2022-03-10T20:42:49+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### 函数为什么是一等公民

> 据类型与函数是很多高级语言中最重要的两个概念，前者用来存储数据，后者用来存储代码。JavaScript 中的函数相对于数据类型而言更加复杂，它可以有属性，也可以被赋值给一个变量，还可以作为参数被传递......正是这些强大特性让它成了 JavaScript 的“一等公民”

#### `this` 关键字

- `this` 指向的应该是一个对象，更具体地说是函数执行的“上下文对象”
- 这个对象指向的是“调用它”的对象，如果调用它的不是对象或对象不存在，则会指向全局对象（严格模式下为 undefined）

```js
function fn() {
  console.log(this);
}
function fn2() {
  fn();
}
var obj = { fn2 };
obj.fn2(); // ?
```

- 调用函数 fn() 的是函数 fn2() 而不是 obj。虽然 fn2() 作为 obj 的属性调用，但 **fn2()中的 this 指向并不会传递给函数 fn()**， 所以答案也是 window（Node.js 下是 global）

---

```js
var dx = {
  arr: [1],
};
dx.arr.forEach(function () {
  console.log(this);
}); // ?
```

- `forEach`，它有两个参数，第一个是回调函数，第二个是 this 指向的对象，这里只传入了回调函数，第二个参数没有传入，默认为 undefined，所以正确答案应该是输出全局对象。
- 类似的，需要传入 this 指向的函数还有：`every()`、`find()`、`findIndex()`、`map()`、`some()`

---

```js
class B {
  fn() {
    console.log(this);
  }
}
var b = new B();
var fun = b.fn;
fun(); // ?
```

- ES6 下的 class 内部默认采用的是严格模式，实际上面代码的类定义部分可以理解为下面的形式

```js
class B {
  'use strict';
  fn() {
    console.log(this);
  }
}
```

- 而严格模式下不会指定全局对象为默认调用对象，所以答案是 `undefined`。

---

- ES6 新加入的箭头函数不会创建自己的 this，它只会从自己的作用域链的上一层继承 this。可以简单地理解为箭头函数的 this 继承自上层的 this，但在全局环境下定义仍会指向全局对象。

```js
var arrow = {
  fn: () => {
    console.log(this);
  },
};
arrow.fn(); // ?
```

- 虽然通过对象 arrow 来调用箭头函数 fn()，**this 指向不是 arrow 对象，而是全局对象**
- 如果要让 fn() 箭头函数指向 arrow 对象，还需要再加一层函数，让箭头函数的上层 this 指向 arrow 对象。

```js
var arrow = {
  fn() {
    const a = () => console.log(this);
    a();
  },
};
arrow.fn(); // arrow
```

---

- 箭头函数和普通函数相比，有以下几个区别，在开发中应特别注意：

  - 不绑定 arguments 对象，也就是说在箭头函数内访问 arguments 对象会报错；
  - 不能用作构造器，也就是说不能通过关键字 new 来创建实例；
  - 默认不会创建 prototype 原型属性；
  - 不能用作 Generator() 函数，不能使用 yeild 关键字。

- 函数的转换
  - 不定参数的求和处理

```js
add(1); // 1
add(1)(2); // 3
add(1, 2)(3, 4, 5)(6); // 21
```

```js
function add(...args) {
  let arr = args;
  function fn(...newArgs) {
    arr = [...arr, ...newArgs];
    return fn;
  }
  fn.toString = fn.valueOf = function () {
    return arr.reduce((acc, cur) => acc + parseInt(cur));
  };
  return fn;
}
```

- `toString()` 函数会在打印函数的时候调用，比如 console.log
- `valueOf()` 会在获取函数原始值时调用，比如加法操作。

---

#### 原型

> 原型是 JavaScript 的重要特性之一，可以让对象从其他对象继承功能特性，原型是 JavaScript 的重要特性之一，可以让对象从其他对象继承功能特性

##### 什么是原型和原型链？

- 原型就是对象的属性，**包括被称为隐式原型的 `proto` 属性 和 被称为显式原型的 `prototype` 属性。**

---

- 隐式原型通常在创建实例的时候就会自动指向构造函数的显式原型。例如，在下面的示例代码中，当创建对象 a 时，a 的隐式原型会指向构造函数 Object() 的显式原型。

```js
var a = {};
a.__proto__ === Object.prototype; // true
var b = new Object();
b.__proto__ === a.__proto__; // true
```

---

- **显式原型**是内置函数（比如 Date() 函数）的默认属性，在自定义函数时（箭头函数除外）也会默认生成，生成的显式原型对象只有一个属性 constructor ，该属性指向函数自身。
- 通常配合 new 关键字一起使用，当通过 new 关键字创建函数实例时，**会将实例的隐式原型指向构造函数的显式原型。**

```js
function fn() {}
fn.prototype.constructor === fn; // true
```

##### `new` 操作符实现了什么？

```js
function F(init) {}
var f = new F(args);
```

1. 创建一个临时的空对象，命名为 fn，让对象 fn 的隐式原型指向函数 F 的显式原型；
2. 执行函数 F()，将 this 指向对象 fn，并传入参数 args，得到执行结果 result；
3. 判断上一步的执行结果 result，如果 result 为非空对象，则返回 result，否则返回 fn。

即：

```js
var fn = Object.create(F.prototype);
var obj = F.apply(fn, args);
var f = obj && typeof obj === 'object' ? obj : fn;
```

##### 怎么通过原型链实现多层继承？

- 假设构造函数 B() 需要继承构造函数 A()，就可以通过将函数 B() 的显式原型指向一个函数 A() 的实例，然后再对 B 的显式原型进行扩展。那么通过函数 B() 创建的实例，既能访问用函数 B() 的属性 b，也能访问函数 A() 的属性 a，从而实现了多层继承。

```js
function A() {}
A.prototype.a = function () {
  return 'a';
};
function B() {}
B.prototype = new A();
B.prototype.b = function () {
  return 'b';
};
var c = new B();
c.b(); // 'b'
c.a(); // 'a'
```

---

#### `typeof`

| 类型            | 结果        |
| --------------- | ----------- |
| Undefined       | "undefined" |
| Boolean         | "boolean"   |
| Number          | "number"    |
| BigInt          | "bigint"    |
| String          | "string"    |
| Symbol          | "symbol"    |
| 函数对象        | "function"  |
| 其他对象及 null | "object"    |

#### `instanceof`

- 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。例如，在表达式 left instanceof right 中，会沿着 left 的原型链查找，看看是否存在 right 的 prototype 对象

`left.__proto__.__proto__... =?= right.prototype`

---

#### 作用域

- 作用域是指赋值、取值操作的执行范围，通过作用域机制可以有效地防止变量、函数的重复定义，以及控制它们的可访问性

- 在浏览器端和 Node.js 端作用域的处理有所不同
  - 比如对于全局作用域，浏览器会自动将未主动声明的变量提升到全局作用域
  - Node.js 则需要显式的挂载到 global 对象上。
  - 比如在 ES6 之前，浏览器不提供模块级别的作用域，
  - Node.js 的 CommonJS 模块机制就提供了模块级别的作用域。但在类型上，可以分为全局作用域（window/global）、块级作用域（let、const、try/catch）、模块作用域（ES6 Module、CommonJS）及函数作用域。

---

- 对于使用 var 关键字声明的变量以及创建命名函数的时候，JavaScript 在解释执行的时候都会将其声明内容**提升到作用域顶部**，这种机制称为“命名提升”。
- 变量的命名提升允许在**同（子）级作用域中**，在变量声明之前进行引用，但要注意，得到的是未赋值的变量。而且仅限 var 关键字声明的变量，对于 let 和 const 在定义之前引用会报错。

```js
// 方式1
var f = function() {...}
// 方式2
function f() {...}
```

- 两种方式对于调用函数方式以及返回结果而言是没有区别的，但根据命名提升的规则，可以得知:
  - 方式 1 创建了一个匿名函数，让变量 f 指向它，这里会发生变量的命名提升；
  - 如果在定义函数之前调用会报错，而方式 2 则不会。

#### 闭包

- 在函数内部访问外部函数作用域时就会产生闭包。
- 它允许将函数与其所操作的某些数据（环境）关联起来。这种关联不只是跨作用域引用，也可以实现数据与函数的隔离。

---

题目：修复下列代码

```js
for (var i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000 * i);
}
```

```js
for (let i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000 * i);
}
/**
等价于
for(var i = 0; i < 5; i++ ) {
    let _i = i
	setTimeout(() => {
		console.log(_i);
	}, 1000 * i)
}
 */
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Object.prototype.valueOf() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)
