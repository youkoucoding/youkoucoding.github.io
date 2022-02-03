---
title: '毎日のフロントエンド  140'
date: 2022-02-03T18:33:02+09:00
description: frontend 每日一练
image: frontend-140-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十九日

## HTML

### **Question:** HTML5 `keygen`标签

`<keygen>` 该特性已经从 Web 标准中删除

HTML `<keygen>` 元素是为了方便生成密钥材料和提交作为 HTML form 的一部分的公钥.这种机制被用于设计基于 Web 的证书管理系统。按照预想，`<keygen>` 元素将用于 HTML 表单与其他的所需信息一起构造一个证书请求，该处理的结果将是一个带有签名的证书。

## CSS

### **Question:** transition 是否可以过渡 opacity 和 display

- `opacity`可以过渡
- `display`不能

[CSS animated properties (可动画属性列表) - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties)

## JavaScript

### **Question:** `Set`, `Map`, `WeakSet` 和 `WeakMap` 的区别

#### `Set`

- ES6 新增的一种新的数据结构，类似于数组，但成员是**唯一且无序的**，没有重复的值
- 允许储存任何类型的唯一值，无论是基本数据类型或引用数据类型
- Set 本身是一种构造函数，用来生成 Set 数据结构 `new Set([iterable])`

```js
const s = new Set()
[1, 2, 3, 4, 3, 2, 1].forEach(x => s.add(x))

for (let i of s) {
    console.log(i)	// 1 2 3 4
}

// 去重数组的重复对象
let arr = [1, 2, 3, 2, 1, 1]
[... new Set(arr)]	// [1, 2, 3]
```

- 向 Set 加入值的时候，不会发生类型转换，所以 5 和"5"是两个不同的值

  - Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（`===`）
  - 主要的区别是**NaN 等于自身，而精确相等运算符认为 NaN 不等于自身**

  ```js
  let set = new Set();
  let a = NaN;
  let b = NaN;
  set.add(a);
  set.add(b);
  set; // Set {NaN}
  ```

#### `WeakSet`

- `WeakSet` 对象允许将**弱引用对象**储存在一个集合中
- `WeakSet` 与 `Set` 的区别：
  - `WeakSet` 只能储存对象引用，不能存放值，而 Set 对象都可以
  - `WeakSet` 对象中储存的对象值都是被弱引用的，即垃圾回收机制不考虑 WeakSet 对该对象的应用，如果没有其他的变量或属性引用这个对象值，则这个对象将会被垃圾回收掉（不考虑该对象还存在于 WeakSet 中），所以，WeakSet 对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了（被垃圾回收了），WeakSet 对象是无法被遍历的（ES6 规定 WeakSet 不可遍历），也没有办法拿到它包含的所有元素

#### `Map`

- `Map` 对象保存键值对，并且能够记住键的原始插入顺序。任何值(对象或者原始值) 都可以作为一个键或一个值。
- `Map` `Set`区别

  - 共同点：可以储存不重复的值
  - 不同点：`Set` 是以 `[value, value]`的形式储存元素，字典 是以 `[key, value]` 的形式储存

- 任何具有 `Iterator` 接口、且每个成员都是一个双元素的数组的数据结构都可以当作 Map 构造函数的参数

```js
const set = new Set([
  ['foo', 1],
  ['bar', 2],
]);
const m1 = new Map(set);
m1.get('foo'); // 1

const m2 = new Map([['baz', 3]]);
const m3 = new Map(m2);
m3.get('baz'); // 3
```

- 如果读取一个未知的键，则返回`undefined`

1. `Map` 转 `Array`

```js
const map = new Map([
  [1, 1],
  [2, 2],
  [3, 3],
]);
console.log([...map]); // [[1, 1], [2, 2], [3, 3]]
```

2. `Array` 转 `Map`

```js
const map = new Map([
  [1, 1],
  [2, 2],
  [3, 3],
]);
console.log(map); // Map {1 => 1, 2 => 2, 3 => 3}
```

3. `Map` 转 `Object`
   - Object 的键名都为字符串，而 Map 的键名为对象，所以转换的时候会把非字符串键名转换为字符串键名。

```js
function mapToObj(map) {
  let obj = Object.create(null);
  for (let [key, value] of map) {
    obj[key] = value;
  }
  return obj;
}
const map = new Map().set('name', 'An').set('des', 'JS');
mapToObj(map); // {name: "An", des: "JS"}
```

4. `Object` 转 `Map`

```js
function objToMap(obj) {
  let map = new Map();
  for (let key of Object.keys(obj)) {
    map.set(key, obj[key]);
  }
  return map;
}

objToMap({ name: 'An', des: 'JS' }); // Map {"name" => "An", "des" => "JS"}
```

5. `Map` 转 `JSON`

```js
function mapToJson(map) {
  return JSON.stringify([...map]);
}

let map = new Map().set('name', 'An').set('des', 'JS');
mapToJson(map); // [["name","An"],["des","JS"]]
```

6. `JSON` 转 `Map`

```js
function jsonToStrMap(jsonStr) {
  return objToMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"name": "An", "des": "JS"}'); // Map {"name" => "An", "des" => "JS"}
```

#### `WeakMap`

- `WeakMap` 对象是一组键值对的集合，其中的键是弱引用对象，而值可以是任意
  - `WeakMap` 弱引用的只是键名，而不是键值。键值依然是正常引用。
- `WeakMap` 中，每个键对自己所引用对象的引用都是弱引用，在没有其他引用和该键引用同一对象，这个对象将会被垃圾回收（相应的 key 则变成无效的），所以，WeakMap 的 key 是不可枚举的。

#### Summary

- `Set`
  - 成员唯一、无序且不重复
  - [value, value]，键值与键名是一致的（或者说只有键值，没有键名）
  - 可以遍历，方法有：add、delete、has
- `WeakSet`
  - 成员都是对象
  - 成员都是弱引用，可以被垃圾回收机制回收，可以用来保存 DOM 节点，不容易造成内存泄漏
  - 不能遍历，方法有 add、delete、has
- `Map`
  - 本质上是键值对的集合，类似集合
  - 可以遍历，方法很多可以跟各种数据格式转换
- `WeakMap`
  - 只接受对象作为键名（null 除外），不接受其他类型的值作为键名
  - 键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾回收，此时键名是无效的
  - 不能遍历，方法有 get、set、has、delete

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/question/)

[Set - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)

[WeakSet - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

[Map - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)

[WeakMap - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
