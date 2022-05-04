---
title: '毎日のフロントエンド 224'
date: 2022-05-04T23:14:56+09:00
description: frontend 每日一练
categories:
  - Tips
---

## JavaScript

### `typeof NaN`

- `typeof NaN` 结果是 'number'
- `==` 和 `===` 不能被用来判断一个值是否是 NaN。必须使用 `Number.isNaN()` 或 `isNaN()` 函数。在执行自比较之中：NaN，也只有 NaN，比较之中不等于它自己

```js
NaN === NaN; // false
Number.NaN === NaN; // false
isNaN(NaN); // true
isNaN(Number.NaN); // true

function valueIsNaN(v) {
  return v !== v;
}
valueIsNaN(1); // false
valueIsNaN(NaN); // true
valueIsNaN(Number.NaN); // true
```

## Tips

### 函数式编程 Tips

> 在函数式编程中，函数就是一个管道（pipe） 输入一个值，就会输出来一个新的值，没有其他作用

#### 函数的合成与柯里化

##### COMPOSE

- 如果一个值要经过多个函数，才能变成另外一个值，就可以把所有中间步骤合并成一个函数，这叫做"函数的合成"（compose）
- 函数的合成还必须满足结合律
- 函数就像数据的管道（pipe）。那么，函数合成就是将这些管道连了起来，让数据从多个管道中穿过

##### CURRY

- 所谓"柯里化"，就是把一个多参数的函数，转化为单参数函数

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [Ramda 函数库参考教程](https://www.ruanyifeng.com/blog/2017/03/ramda.html)

- [函数式编程入门教程](https://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html)

- [函数式编程指北](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/)
