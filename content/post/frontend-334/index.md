---
title: '毎日のフロントエンド 334'
date: 2022-06-13T22:03:33+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `<map>` `<area>`

- `<map>` 属性 与 `<area>` 属性一起使用来定义一个图像映射 (一个可点击的链接区域)

## CSS

### `direction`

- CSS 属性 direction 用来**设置文本、表列水平溢出的方向**。 rtl 表示从右到左 (类似希伯来语或阿拉伯语)， ltr 表示从左到右 (类似英语等大部分语言).

## JS

### `JSON.stringify()`

#### 1. 对于 _undefined_、*任意的函数*以及 _symbol_ 三个特殊的值分别作为对象属性的值、数组元素、单独的值时 JSON.stringify()将返回不同的结果

```js
const data = {
  a: 'aaa',
  b: undefined,
  c: Symbol('dd'),
  fn: function () {
    return true;
  },
};
JSON.stringify(data); // 输出： "{"a":"aaa"}"
```

- undefined、任意的函数以及 symbol 作为对象属性值时 JSON.stringify() 将跳过（忽略）对它们进行序列化

---

```js
JSON.stringify([
  'aaa',
  undefined,
  function aa() {
    return true;
  },
  Symbol('dd'),
]); // 输出："["aaa",null,null,null]"
```

- undefined、任意的函数以及 symbol 作为**数组元素值**时，JSON.stringify() 会将它们序列化为 `null`

---

```js
JSON.stringify(function a() {
  console.log('a'); // undefined
});
JSON.stringify(undefined); // undefined
JSON.stringify(Symbol('dd')); // undefined
```

- undefined、任意的函数以及 symbol 被 _JSON.stringify() 作为单独的值进行序列化时都会返回 undefined_

#### 2. 非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中

```js
const data = {
  a: 'aaa',
  b: undefined,
  c: Symbol('dd'),
  fn: function () {
    return true;
  },
  d: 'ddd',
};
JSON.stringify(data); // 输出："{"a":"aaa","d":"ddd"}"

JSON.stringify([
  'aaa',
  undefined,
  function aa() {
    return true;
  },
  Symbol('dd'),
  'eee',
]); // 输出："["aaa",null,null,null,"eee"]"
```

#### 3. 转换值如果有 `toJSON()` 函数，该函数返回什么值，序列化结果就是什么值，并且忽略其他属性的值

```js
JSON.stringify({
  say: 'hello JSON.stringify',
  toJSON: function () {
    return 'today i learn';
  },
}); // "today i learn"
```

#### 4. JSON.stringify() 将会正常序列化 `Date` 的值

```js
JSON.stringify({ now: new Date() });
//'{"now":"2022-06-14T12:03:49.262Z"}'
```

- Date 对象自己部署了 toJSON() 方法（同 Date.toISOString()），因此 Date 对象会被当做字符串处理

---

#### 5. NaN 和 Infinity 格式的数值及 null 都会被当做 null

```js
JSON.stringify(NaN);
// "null"
JSON.stringify(null);
// "null"
JSON.stringify(Infinity);
// "null"
```

#### 6. 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值

```js
JSON.stringify([new Number(1), new String('false'), new Boolean(false)]); // "[1,"false",false]"
```

#### 7. 其他类型的对象，包括 Map/Set/WeakMap/WeakSet，_仅会序列化可枚举的属性_

```js
// 不可枚举的属性默认会被忽略：
JSON.stringify(
  Object.create(null, {
    x: { value: 'json', enumerable: false },
    y: { value: 'stringify', enumerable: true },
  })
);
// "{"y":"stringify"}"
```

#### 8. 实现深拷贝最简单的方式就是序列化：JSON.parse(JSON.stringify())，这个方式实现深拷贝会因为序列化的诸多特性从而导致诸多的缺点

#### 9. 以 symbol 为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们

```js
JSON.stringify({ [Symbol.for('json')]: 'stringify' }, function (k, v) {
  if (typeof k === 'symbol') {
    return v;
  }
}); // undefined
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTML 图片热区 map area 的用法 ](https://www.cnblogs.com/mq0036/p/3337327.html)

- [`<area>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/area)

- [`<map>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/map)

- [direction - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/direction)
