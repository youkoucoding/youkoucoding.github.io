---
title: '毎日のフロントエンド　 71'
date: 2021-11-26T17:22:43+09:00
description: frontend 每日一练
image: frontend-71-cover.jpg
categories:
  - Javascript
---

# 第七十一日

## JavaScript

### **Question:** 深拷贝对象

- 使用原生的 `Object.assign()`,以及展开运算符，以实现**浅拷贝**

```js
/**
 * 深度拷贝一个对象
 * @param {any} val
 */
function deepClone(val) {
  function getType(_val) {
    return (
      Object.prototype.toString
        .call(_val)
        // 对_val调用toString方法,比如:一个number调用该方法后,会得到:
        // "[object Number]"
        // 然后替换其中的object
        // 具体调用示例如下:
        //  Object.prototype.toString.call(1)
        //  "[object Number]"
        //  Object.prototype.toString.call({})
        //  "[object Object]"
        //  Object.prototype.toString.call([])
        //  "[object Array]"
        //  Object.prototype.toString.call('')
        //  "[object String]"
        //  Object.prototype.toString.call(true)
        //  "[object Boolean]"
        //  Object.prototype.toString.call(Symbol())
        //  "[object Symbol]"
        //  Object.prototype.toString.call(()=>{})
        //  "[object Function]"
        //  Object.prototype.toString.call(undefined)
        //  "[object Undefined]"
        //  Object.prototype.toString.call(null)
        //  "[object Null]"
        .replace(/[\[\s\]]|(object)/g, '')
    );
  }
  const valType = getType(val);
  // 如果是非引用类型,直接赋值就是深度拷贝了
  if (valType !== 'Object' && valType !== 'Array') return val;
  let output = {};
  // 如果是 Array 类型的话,新建一个Array, 然后将值拷贝到数组中,(递归)
  if (valType === 'Array') {
    output = [];
    val.forEach((v) => {
      output.push(v);
    });
  } else if (valType === 'Object') {
    Object.keys(val).forEach((v) => {
      output[v] = deepClone(val[v]);
    });
  } else if (valType === 'Function') {
    // 针对函数,使用assign来实现
    output[val.name] = Object.assign({}, val);
  }
  return output;
}
```

### **Question:** 判断输出结果

```js
const num = {
  a: 10,
  add() {
    return this.a + 2;
  },
  reduce: () => this.a - 2,
};
console.log(num.add());
console.log(num.reduce());
```

Result: 12 NaN

add 是普通函数，而 reduce 是箭头函数。对于箭头函数，this 关键字指向是它所在上下文（**定义时的位置**）的环境，与普通函数不同！ 这意味着当我们调用 reduce 时，它不是指向 num 对象，而是指其定义时的环境（`window`）。没有值 a 属性，返回 undefined。

---

```js
const person = { name: 'yideng' };

function sayHi(age) {
  return `${this.name} is ${age}`;
}
console.log(sayHi.call(person, 21));
console.log(sayHi.bind(person, 21));
```

Result:

- `yideng is 21`
- ƒ sayHi(age) { return `${this.name} is ${age}`; }

使用两者，以传递我们想要`this`关键字引用的对象:

- `.call`方法会立即执行
- `.bind`方法会返回函数的拷贝值，带有绑定的函数执行上下文， 它不会立即执行

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
