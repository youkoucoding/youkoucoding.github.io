---
title: '毎日のフロントエンド  88'
date: 2021-12-13T10:17:48+09:00
description: frontend 每日一练
image: frontend-88-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十八日

## HTML

### **Question:** 怎么使用`HTML5`来获取定位？定位不准怎么解决

- `navigator.geolocation` [使用地理位置定位 - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation_API)

## CSS

### **Question:** CSS 样式覆盖规则

1. 首先看权重，权重高的样式会覆盖权重低大的样式。`!important` > `#id` > `.class` > `tag` > `*`
2. 同等权重时，`css` 靠后的覆盖靠前的（**就近原则**），只与 css 书写的顺序有关，与 class 引用的顺序无关
3. 行内样式 > 内联样式 > 外联样式

## JavaScript

### **Question:** 判断 `instanceof` 的结果并解释原因

```js
function test() {
  return test;
}
new test() instanceof test;
```

Result: `false`

- `instanceof` 运算符将检测*右端值*的 `prototype` 属性是否在左端值的原型链（`[[Prototype]]`(`__proto__`) 属性）上
- 如果不在，则向上查找（`[[Prototype]]` 的 `[[Prototype]]`，…），直到找遍左端值的整个原型链。

---

- 如果函数明确返回 `non-primitive`(即，引用类型) 值，那么 `new` 运算符的结果将是这个值，也就是说，原式相当于`test instanceof test`

- _左端值_ `test` 是一个 `Function`，故它的原型链为：`Function.prototype -> Object.prototype -> null `
- 此原型链上没有 `test.prototype` 出现，所以 test 并不是 test 的一个实例。因此返回`false`

### **Question:** 对于扩展运算符，下面代码的执行结果是什么？并解释原因

```js
let oneObject = { ...null, ...undefined };
console.log(oneObject);
let twoArray = [...null, ...undefined];
console.log(twoArray);
```

Result:

```js
let oneObject = { ...null, ...undefined };
console.log(oneObject); //Object { } 空对象
let twoArray = [...null, ...undefined];
console.log(twoArray); // Uncaught TypeError: null is not iterable (undefined is not iterable)
```

- 第一个打印一个空对象
- 第二个打印报错，是因为在数组和函数中使用展开语法时，**只能用于可迭代对象**

### **Question:** 写出类数组转换结果，并解释原因

```js
const arrLike = {
  length: 4,
  0: 0,
  1: 1,
  '-1': 2,
  3: 3,
  4: 4,
};
console.log(Array.from(arrLike));
console.log(Array.prototype.slice.call(arrLike));
```

Result:

```js
[0, 1, undefined, 3];
[0, 1, empty, 3];
```

- `Array.from()`[^1]: 对一个**类似数组或可迭代对象**创建一个新的，浅拷贝的数组实例。

- `Array.prototype.slice.call()`[^2]:

  1. **返回一个新的数组对象**,这一对象是一个由 `begin` 和 `end` 决定的原数组的浅拷贝（包括 begin，不包括 end）。原始数组不会被改变。
  2. 方法可以用来将一个类数组（Array-like）对象**转换成一个新数组**。只需将该方法绑定到这个对象上。

[^1]: [Array.from() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
[^2]: [Array.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

- 伪数组对象(类数组对象)：拥有一个 `length` 属性和若干索引属性的任意对象, 一个函数中的 `arguments` 就是一个类数组对象的例子。(arguments 在 ES6 中，被 rest 参数代替了)

Example:

```javascript
function list() {
  return Array.prototype.slice.call(arguments);
}
var list1 = list(1, 2, 3); // [1, 2, 3]
```

#### 类数组

类数组的定义，有如下两条：

1. 具有：指向对象元素的数字索引下标以及 `length` 属性表明元素个数

2. 不具有：如`push`、`forEach` 以及 `indexOf` 等数组对象具有的方法

三个典型的 JavaScript 类数组:

1. `DOM`方法

```js
// 获取所有div
let arrayLike = document.querySelectorAll('div');

console.log(Object.prototype.toString.call(arrayLike)); // [object NodeList]

console.log(arrayLike.length); // 127

console.log(arrayLike[0]);
// <div id="js-pjax-loader-bar" class="pjax-loader-bar"></div>

console.log(Array.isArray(arrayLike)); // false

arrayLike.push('push');
// Uncaught TypeError: arrayLike.push is not a function(…)
```

2. 类数组对象

```js
let arrayLikeObj = {
  length: 2,
  0: 'This is Array Like Object',
  1: true,
};

console.log(arrayLikeObj.length); // 2
console.log(arrayLikeObj[0]); // This is Array Like Object
console.log(Array.isArray(arrayLikeObj)); // false

let arrObj = Array.prototype.slice.call(arrayLikeObj, 0);
console.log(Array.isArray(arrObj)); // true
```

3. 类数组函数

```js
let arrayLikeFunc1 = function () {};
console.log(arrayLikeFunc1.length); // 0
let arrFunc1 = Array.prototype.slice.call(arrayLikeFunc1, 0);
console.log(arrFunc1, arrFunc1.length); // ([], 0)

let arrayLikeFunc2 = function (a, b) {};
console.log(arrayLikeFunc2.length); // 2
let arrFunc2 = Array.prototype.slice.call(arrayLikeFunc2, 0);
console.log(arrFunc2, arrFunc2.length); // ([undefined × 2], 2)
```

> `length>0`时，如果转为数组，则数组里的值会是`undefined`。个数等于函数`length`的长度。

#### 类数组的实现原理

1. `JS` “万物皆对象”概念

> 在 JavaScript 中，“一切皆对象”，数组和函数本质上都是对象，就连三种原始类型的值——数值、字符串、布尔值——在一定条件下，也会自动转为对象，也就是原始类型的“包装对象”。

2. `JS` 支持的“鸭子类型”

> 对象/函数，但是只要有 length 属性和对应的数字下标，那么他们就是数组

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[Array.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#array-like)

[深入理解 JavaScript 类数组 - SegmentFault 思否](https://segmentfault.com/a/1190000005076858)
