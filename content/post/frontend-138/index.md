---
title: '毎日のフロントエンド  138'
date: 2022-02-01T11:57:11+09:00
description: frontend 每日一练
image: frontend-138-cover.jpg
categories:
  - Javascript
---

# 第一百三十八日

## Tips

### 深・浅拷贝

#### 浅拷贝

> 创建一个新对象， 接收重新复制或引用的对象值。 如果对象属性是基本数据类型，复制的就是基本数据类型的值给新对象； 如果属性值是引用数据类型，复制的就是引用对象在内存中的地址。 如果一个对象改变了此地址上对象的内容，必然影响另外一个。

1. `Object.assign(target, ...sources)`

   ```js
   let target = {};
   let source = { a: { b: 2 } };
   Object.assign(target, source);

   console.log(target); // {a:{b:10}}
   source.a.b = 10;
   consolo.log(source); // {a:{b:10}}
   console.log(target); // {a:{b:10}}
   ```

   - 不会拷贝对象的继承属性
   - 不会拷贝对象的**不可枚举属性**
   - 可以拷贝 `symbol` 类型属性

   - 针对深拷贝，需要使用其他办法，因为 Object.assign()拷贝的是（可枚举）属性值。假如源值是一个对象的引用，它仅仅会复制其引用值。

2. 扩展运算符

在构造对象的同时，完成浅拷贝的功能

- `let cloneObj = {...obj}`
- 实际上, 展开语法和 Object.assign() 行为一致, 执行的都是浅拷贝(只遍历一层)。

3. `concat` 拷贝数组

- `concat()` 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。
- 它不会递归到嵌套数组参数中。

4. `slice` 拷贝数组

- `arr.slice(begin, end)`
- `slice()` 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括 end）。原始数组不会被改变。

5. 手工实现一个浅拷贝

- 对基本数据类型的数据，做拷贝
- 对引用对象，在新存储位置上，拷贝一层对象属性

```js
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? [] : {};

    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = target[prop];
      }
    }
    return cloneTarget;
  }
  return target;
};
```

#### 深拷贝

> 开辟了新的内存， 将原对象，完全复制进新地址， 两个对象，相互独立，互不影响。

1. 方法一 `JSON.stringify`

   ```js
   let obj1 = { a: 1, b: [1, 2, 3] };
   let str = JSON.stringify(obj1);
   let obj2 = JSON.parse(str);
   ```

   Caveat:

   1. 拷贝的对象的值中，如果有，函数，undefined，symbol， 经过`JSON.stringify()`序列化之后，这个键值对会消失
   2. 拷贝`Date`引用类型，会变成字符串
   3. 无法拷贝**不可枚举对象**
   4. 无法拷贝对象的原型链
   5. 拷贝正则引用类型， 会变成空对象
   6. 对象中有，`NaN`, `Infinty`,`-Infinty`, 经过序列化之后，结果会变为`null`
   7. 无法拷贝对象的循环引用， 即对象成环: `obj[key]:obj`

2. 递归实现， 基础实现

   ```js
   let obj = { a: { b: 1 } };

   const deepClone = (obj) => {
     let cloneObj = {};
     for (key in obj) {
       if (typeof obj[key] === 'object') {
         cloneObj[key] = deepClone(obj[key]);
       } else {
         cloneObj[key] = obj[key];
       }
     }
     return cloneObj;
   };
   ```

   Caveat:

   - 这个递归函数，不能复制，**不可枚举属性**以及 `symbol`类型
   - 这种方法只能针对，普通引用类型的值进行递归复制
   - 循环引用问题未解决

3. 改进的递归实现

   - `Reflect.ownKeys` 解决： **不可枚举属性**以及 `symbol`类型
   - 当属性值为，`Date`, `RegExp`，直接返回新的实例
   - 利用`getOwnPropertyDescriptors`，可以获得对象所有属性，以及其对应特性， 结合`create()`方法创建一个新对象，并继承传入对象的原型链
   - 利用`WeakMap`类型作为`hash table`，weakmap 是弱引用类型，可防止内存泄漏，用于检测循环引用： 如果存在循环引用，则返回`WeakMap`存储的值

   ```js
   const isComplexDataType = (obj) =>
     (typeof obj === 'object' || typeof obj === 'function') && obj !== null;

   const deepClone = function (obj, hash = new WeakMap()) {
     /* Date 类型，直接返回一个新的实例*/
     if (obj.constructor === Date) return new Date(obj);

     if (obj.constructor === RegExp) return new RegExp(obj);

     /*  有循环引用， 引入 weakmap*/
     if (hash.has(obj)) return hash.get(obj);

     let allDesc = Object.getOwnPropertyDescriptors(obj);

     /* 遍历传入所有 键的 特征 */
     let cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc);

     /* 继承原型链*/
     hash.set(obj, cloneObj);
     for (let key of Reflect.ownKeys(obj)) {
       cloneObj[key] =
         isComplexDataType(obj[key]) && typeof obj[key] !== 'function'
           ? deepClone(obj[key], hash)
           : obj[key];
     }
     return cloneObj;
   };
   ```

   ```js
   // test
   let obj = {
     num: 0,
     str: '',
     boolean: true,
     unf: undefined,
     null: null,
     obj: { id: 12345, name: 'Who I am' },
     arr: [1, 2],
     func: function () {},
     date: new Date(0),
     reg: new RegExp('/123/ig'),
     [Symbol('1')]: 1,
   };

   Object.defineProperty(obj, 'innumerable', { enumerable: false, value: '不可枚举属性' });

   /*  设置循环引用 */
   obj = Object.create(obj, Object.getOwnPropertyDescriptors(obj));
   obj.loop = obj;

   let cloneObj = deepClone(obj);

   cloneObj.arr.push(12345);

   console.log('source', obj);
   console.log('result', cloneObj);
   ```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Object.assign() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

[展开语法 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

[Array.prototype.concat() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

[Array.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

[Object.prototype.hasOwnProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

[Object.defineProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

[Object.getOwnPropertyDescriptor() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

[WeakMap - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

[Object.getOwnPropertyDescriptors() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)
