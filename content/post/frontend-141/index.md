---
title: '毎日のフロントエンド  141'
date: 2022-02-04T15:05:53+09:00
description: frontend 每日一练
image: frontend-141-cover.jpg
categories:
  - Javascript
---

# 第一百四十一日

## JavaScript

### **Question:** 一个洗扑克牌的方法

```js
let Shuffle = () =>
  [...'A23456789JQK', 10]
    .reduce((arr, e) => arr.concat([`♥${e}`, `♠${e}`, `♣${e}`, `♦${e}`]), ['带🃏', '小🃏'])
    .sort(() => Math.floor(Math.random() * 2 - Math.random() * 2));
```

### **Question:** 以下 3 个判断数组的方法，区别和优劣

1. `Object.prototype.toString.call()`
   - 每一个继承 Object 的对象都有 toString 方法，如果 toString 方法没有重写的话，会返回 [Object type]，其中 type 为对象的类型。但当除了 Object 类型的对象外，其他类型直接使用 toString 方法时，会直接返回都是内容的字符串，所以使用 call 或者 apply 方法来改变 toString 方法的执行上下文。

```js
const an = ['Hello', 'An'];
an.toString(); // "Hello,An"
Object.prototype.toString.call(an); // "[object Array]"
```

- 对于所有基本的数据类型都能进行判断，包括 `null` 和 `undefined`

```js
Object.prototype.toString.call('An'); // "[object String]"
Object.prototype.toString.call(1); // "[object Number]"
Object.prototype.toString.call(Symbol(1)); // "[object Symbol]"
Object.prototype.toString.call(null); // "[object Null]"
Object.prototype.toString.call(undefined); // "[object Undefined]"
Object.prototype.toString.call(function () {}); // "[object Function]"
Object.prototype.toString.call({ name: 'An' }); // "[object Object]"
```

2. `instanceof`

   - `instanceof` 的内部机制是通过判断对象的原型链中是不是能找到类型的 `prototype`
   - 使用 `instanceof`判断一个对象是否为数组，instanceof 会判断这个对象的原型链上是否会找到对应的 Array 的原型，找到返回 true，否则返回 false
   - instanceof 只能用来判断对象类型，**原始类型不可以**。并且所有对象类型 instanceof Object 都是 true

3. `Array.isArray()`
   - 用来判断对象是否为数组
   - `instanceof` 与 `isArray`
     - 当检测 Array 实例时，**`Array.isArray` 优于 `instanceof`** ，因为 Array.isArray 可以检测出 iframes
   - `Array.isArray()` 与 `Object.prototype.toString.call()`
     - Array.isArray()是 ES5 新增的方法，当不存在 Array.isArray() ，可以用 Object.prototype.toString.call() 实现
     ```js
     // polyfill
     if (!Array.isArray) {
       Array.isArray = function (arg) {
         return Object.prototype.toString.call(arg) === '[object Array]';
       };
     }
     ```

---

```js
var a = [];
// 1.基于instanceof
a instanceof Array;
// 2.基于constructor
a.constructor === Array;
// 3.基于Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a);
// 4.基于getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype;
// 5.基于Object.prototype.toString
Object.prototype.toString.apply(a) === '[object Array]';
```

## 深入 js 数组

1. `Array` 构造函数
   - `new Array()` 参数为 0 或者 大于 2 时，传入的参数将按照顺序依次成为新数组的第 0 至第 N 项（参数长度为 0 时，返回空数组）
   - `new Array(len)`，当 len 不是数值时，处理同上，返回一个只包含 len 元素一项的数组；当 len 为数值时，len 最大不能超过 32 位无符号整型，即需要小于 2 的 32 次方（len 最大为 Math.pow(2,32)），否则将抛出 RangeError。

- ES6 新增的构造方法：`Array.of` 和 `Array.from`

  - `Array.of` 用于将参数依次转化为数组中的一项，然后**返回这个新数组**
    - 基本上与 Array 构造器功能一致，唯一的区别就在单个数字参数的处理上
      - 当参数为两个时，返回的结果是一致的；
      - 当参数是一个时，`Array.of` 会把参数变成数组里的一项，而构造器则会生成长度和第一个参数相同的空数组。
        ```js
        Array.of(8.0); // [8]
        Array(8.0); // [empty × 8]
        Array.of(8.0, 5); // [8, 5]
        Array(8.0, 5); // [8, 5]
        Array.of('8'); // ["8"]
        Array('8'); // ["8"]
        ```
  - `Array.from` 快速便捷地基于其他对象创建新数组，准确来说就是从一个类似数组的可迭代对象中创建一个新的数组实例

    - 只要一个对象有迭代器，Array.from 就能把它变成一个数组（注意：**是返回新的数组，不改变原对象**）
    - Array.from 拥有 3 个参数：

      1. 类似数组的对象，必选；
      2. 加工函数，新生成的数组会经过该函数的加工再返回(可选)；
      3. this 作用域，表示加工函数执行时 this 的值(可选)

      ```js
      var obj = { 0: 'a', 1: 'b', 2: 'c', length: 3 };
      Array.from(
        obj,
        function (value, index) {
          console.log(value, index, this, arguments.length);
          return value.repeat(3); //必须指定返回值，否则返回 undefined
        },
        obj
      );
      ```

      ```js
      a 0 { '0': 'a', '1': 'b', '2': 'c', length: 3 } 2
      b 1 { '0': 'a', '1': 'b', '2': 'c', length: 3 } 2
      c 2 { '0': 'a', '1': 'b', '2': 'c', length: 3 } 2
      [ 'aaa', 'bbb', 'ccc' ]
      ```

      - 除了上述 obj 对象以外，拥有迭代器的对象还包括 String、Set、Map 等

      ```js
      // String
      Array.from('abc'); // ["a", "b", "c"]
      // Set
      Array.from(new Set(['abc', 'def'])); // ["abc", "def"]
      // Map
      Array.from(
        new Map([
          [1, 'ab'],
          [2, 'de'],
        ])
      );
      // [[1, 'ab'], [2, 'de']]
      ```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/question/)

[Array.from() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

[String.prototype.repeat() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

[Object.prototype.isPrototypeOf() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf)
