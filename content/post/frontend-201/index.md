---
title: '毎日のフロントエンド 201'
date: 2022-04-09T17:15:55+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `<p> </p>`会换两行

```css
p {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
}
```

- 以上为`p`的用户代理
  - 默认上下 `margin` 为 `1em`，所以等效于两行字那么高。
  - `margin-block-start /end`等效于上下外边距
  - `margin-inline-start/end`等效于左右外边距

## CSS

### 伪类`:focus-within`的用法

- `:focus-within` 是一个 CSS 伪类 ，表示一个元素获得焦点，或，该元素的后代元素获得焦点。换句话说，元素自身或者它的某个后代匹配 :focus 伪类。

```css
/* 当 <div> 的某个后代获得焦点时，匹配 <div> */
div:focus-within {
  background: cyan;
}
```

## JavaScript

### `Object` 与 `Map` 的异同及使用场景

#### Map

- Map 是一种数据结构（是一种抽象的数据结构类型），数据一对对进行存储，其中包含键以及映射到该键的值。并且由于**键的唯一性，因此不存在重复的键值对。**
- **Map 中的键和值可以是任何数据类型，不仅限于字符串或整数**

#### Object

- JavaScript 中的常规对象是一种字典类型的数据结构——这意味着它依然遵循与 Map 类型相同键值对的存储结构。
- Object 中的 key，或者我们可以称之为**属性**，同样是独一无二的并且对应着一个单独的 value。
- JavaScript 中的 Object 拥有**内置原型(prototype)**
- JavaScript 中几乎所有对象都是 Object 实例，包括 Map

---

- Object 和 Map 的本质都是以键值对的方式存储数据，但实质上他们之间存在很大的区别：
  - 键：
    - `Object`遵循普通的字典规则，键必须是单一类型，并且只能是**整数**、**字符串**或是**`Symbol`类型**。
    - `Map` 中，`key` 可以为**任意数据类型**（包括`Object`, `Array` 等）
  - 元素顺序：Map 会保留所有元素的顺序，而 Object 并不会保证属性的顺序。
  - 继承：Map 是 Object 的实例对象，而 Object 显然不可能是 Map 的实例对象。

```js
var map = new Map([
  [1, 2],
  [3, 4],
]);
console.log(map instanceof Object); //true
var obj = new Object();
console.log(obj instanceof Map); //false
```

#### 如何创建

##### `Map`

- 创建 Map 只有一种方式，使用其内置的构造函数以及`new`。 `Map([iterable])`

```js
var map = new Map(); //Empty Map
var map = new Map([
  [1, 2],
  [2, 3],
]); // map = {1=>2, 2=>3}
```

##### `Object`

1. 直接定义

```js
var obj = {}; //空对象
var obj = { id: 1, name: 'Test object' };
//2 keys here: id maps to 1, and name maps to "Test object"
```

2. 使用构造方法

```js
var obj = new Object(); //空对象
```

3. `Object.prototype.create()`

```js
var obj = Object.create(null); //空对象
```

- 希望继承某个原型对象，而无需定义它的构造函数，如下：

```js
var Vehicle = {
  type: 'General',
  display: function () {
    console.log(this.type);
  },
};
var Car = Object.create(Vehicle); //创建一个继承自 Vehicle 的对象 Car
Car.type = 'Car'; //重写 type 属性
Car.display(); //Car
Vehicle.display(); //General
```

- 尽量避免使用构造函数的方式:
  - 构造函数会写更多代码
  - 性能更差
  - 更加混乱更容易引起程序错误

#### 访问元素

##### `Map`

- Map，获取元素要通过方法`Map.prototype.get(key)`实现
- Object 类似，要获取到值必须先知道所对应的 key/property，不过使用不同的语法：`Object.<key>` or `Object[‘key’]`

```js
obj.id; //1
obj['id']; //1
```

- 判断 Map 中是否存在某个 key `map.has(1);//return boolean value: true/false`
- Object 需要一些额外的判断:

```js
var isExist = obj.id === undefined;
// or
var isExist = 'id' in obj; // 该方法会检查继承的属性
```

- 可以使用`Object.prototype.hasOwnProperty()`判断 Object 中是否存在特定的 key，它的返回值为 true/false，并且**只会检查对象上的非继承属性。**

#### 插入

- Map 支持通过 `Map.prototype.set()`方法插入元素，该方法接收两个参数：`key`，`value`。如果传入已存在的 key，则将会重写该 key 所对应的 value
- Object 添加属性可以使用下面的方法

```js
obj['gender'] = 'female'; //{id: 1, name: "test", gender: "female"}
obj.gender = male; // 重写已存在的属性
//{id: 1, name: "test", gender: "male"}
```

#### 删除

- `Object`并没有提供删除元素的内置方法，可以使用 `delete` 语法：

  - delete 会完全删除 Object 上某个特有的属性
  - `obj[key] = undefined`, 只会改变这个 key 所对应的 value 为 undefined，而该属性仍然保留在对象中, 在使用 `for...in...`循环时**仍然会遍历到该属性的 `key`**。
  - `delete`操作符的返回值为`true/false`，但其返回值的依据与预想情况有所差异：
    - 对于所有情况都返回 true
    - 除非属性是一个`non-configurable`属性，否则在非严格模式返回 false，严格模式下将抛出异常

- `Map` 有更多内置的删除元素方式
  - `delete(key)`用于从 Map 中删除特定 key 所对应的 value，该方法返回一个布尔值
    - 如果目标对象中存在指定的 key 并成功删除，则返回 true；
    - 如果对象中不存在该 key 则返回 false。
  - `clear()` 清空 Map 中所有元素(Object 要实现 Map 的 clear()方法，需要遍历这个对象的属性，逐个删除)

---

- Object 和 Map 删除元素的方法也相似。其中删除某个元素的时间复杂度为 O(1)，清空元素的时间复杂度为 O(n)，n 为 Object 和 Map 的大小。

#### 获取大小

- `Map`的一个优点是它可以自动更新其大小，可以通过以下方式获得：`map.size`
- `Object`，需要通过`Object.keys()`方法计算其大小，该方法**返回一个包含所有 key 的数组。**

```js
console.log(Object.keys(obj).length);
```

#### 迭代

- **Map 有内置的迭代器，Object 没有内置的迭代器**

```js
// 判断某种类型是否可迭代，可以通过以下方式实现
// typeof <obj>[Symbol.iterator] === “function”
console.log(typeof obj[Symbol.iterator]); //undefined
console.log(typeof map[Symbol.iterator]); //function
```

---

- `Map`，所有元素可以通过`for...of`方法遍历：

```js
//For map: { 2 => 3, 4 => 5 }
for (const item of map) {
  console.log(item);
  //Array[2,3]
  //Array[4,5]
}
//Or
for (const [key, value] of map) {
  console.log(`key: ${key}, value: ${value}`);
  //key: 2, value: 3
  //key: 4, value: 5
}
```

- `Map`内置的 forEach()方法

```js
map.forEach((value, key) => console.log(`key: ${key}, value: ${value}`));
//key: 2, value: 3
//key: 4, value: 5
```

- `Object`，使用`for...in`方法

```js
//{id: 1, name: "test"}
for (var key in obj) {
  console.log(`key: ${key}, value: ${obj[key]}`);
  //key: id, value: 1
  //key: name, value: test
}
```

- `Object` 使用`Object.keys(obj)`只能获取所有 key 并进行遍历

```js
Object.keys(obj).forEach((key) => console.log(`key: ${key}, value: ${obj[key]}`));
//key: id, value: 1
//key: name, value: test
```

#### 应用场景

- Map 相对于 Object 有很多优点，依然存在某些使用 Object 会更好的场景:
  - 如果知道所有的`key`，它们都为**字符串**或**整数**（或是**Symbol 类型**），需要一个简单的结构去存储这些数据，Object 是一个非常好的选择。构建一个 Object 并通过知道的特定 key 获取元素的性能要优于 Map（字面量 vs 构造函数，直接获取 vs get()方法）
  - 如果需要在对象中保持自己独有的逻辑和属性，只能使用 Object

```js
// Map并不能实现这样的数据结构
var obj = {
  id: 1,
  name: "It's Me!",
  print: function () {
    return `Object Id: ${this.id}, with Name: ${this.name}`;
  },
};
console.log(obj.print()); //Object Id: 1, with Name: It's Me.
```

- JSON 直接支持 Object，但尚未支持 Map。因此，在某些必须使用 JSON 的情况下，应将 Object 视为首选。
- Map 是一个纯哈希结构，而 Object 不是（它拥有自己的内部逻辑）。使用 delete 对 Object 的属性进行删除操作存在很多性能问题。所以，**针对于存在大量增删操作的场景，使用 Map 更合适。**
- 不同于 Object，Map 会保留所有元素的顺序。Map 结构是在基于可迭代的基础上构建的，所以如果考虑到元素迭代或顺序，使用 Map 更好，它能够确保在所有浏览器中的迭代性能。
- Map 在存储大量数据的场景下表现更好，尤其是在 key 为未知状态，并且所有 key 和所有 value 分别为相同类型的情况下

### TypeScript Tips

- Decode some URL search params **AT THE TYPE LEVEL**
  - 用类型解码 URL 搜索参数, 转化为一个对象

```ts
import { String, Union } from 'ts-toolbelt';

const query = `/home?a=foo&b=bar`;

//convert to below
const obj: Union.Merge<QueryParams> = {
  a: 'foo',
  b: 'bar',
};
```

- Solution

```ts
import { String, Union } from 'ts-toolbelt';

const query = `/home?a=foo&b=bar`;

type Query = typeof query;

type SecondQueryPart = String.Split<Query, '?'>[1];

type QueryElements = String.Split<SecondQueryPart, '&'>;

type QueryParams = {
  [QueryElement in QueryElements[number]]: {
    [Key in String.Split<QueryElement, '='>[0]]: String.Split<QueryElement, '='>[1];
  };
}[QueryElements[number]];

const obj: Union.Merge<QueryParams> = {
  a: 'foo',
  b: 'bar',
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[:focus-within - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus-within)

[Object 与 Map 的异同及使用场景](https://juejin.cn/post/6844903792094232584)

[ES6 — Map vs Object — What and when? | by Maya Shavin | Frontend Weekly | Medium](https://medium.com/front-end-weekly/es6-map-vs-object-what-and-when-b80621932373)

[ts-toolbelt/Split.ts at master · millsp/ts-toolbelt](https://github.com/millsp/ts-toolbelt/blob/master/sources/String/Split.ts)
