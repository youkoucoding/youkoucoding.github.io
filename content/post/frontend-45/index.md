---
title: '毎日のフロントエンド　45'
date: 2021-10-31T10:35:03+09:00
description: frontend 每日一练
image: frontend-45-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十五日

## HTML

### **Question:** `xml`与`html`有什么区别

1. `html`不区分大小写，`xml`区分大小写
2. `html`可以没有闭合标签，`xml`必须有闭合标签
3. `html`可以拥有不带值的属性名，`xml`中所有的属性必须带值
4. `html`是用于显示数据，`xml`主要用于描述，存放数据

## CSS

### **Question:** 等高布局有多少种

1. `flex`拉伸

```css
display: flex;
align-items: stretch;
```

2. `padding margin`抵消 然后`background-clip`默认是`border-box`所以会在被抵消的位置依然显示背景 造成等高假象

```css
.box,
.box2 {
  float: left;
  width: 100px;
}
.box {
  background: #cccccc;
  height: 300px;
}
.box2 {
  background: #306eff;
  padding-bottom: 99999px;
  margin-bottom: -99999px;
}
```

## JavaScript

### **Question:** 写出几种创建对象的方式，区别是什么

#### `new Object()`

```js
var obj = new Object();
var obj = {}; // 等价
```

#### 工厂模式

优点是 可以解决创建多个相似对象的问题，缺点是 无法识别对象的类型

```js
function createObj(name, age) {
  var obj = {};
  obj.name = name;
  obj.age = age;
  return obj;
}
var Anson = createObj('Anson', 18);
console.log(Anson); //{name: "Anson", age: 18}
```

#### 构造函数

优点是 可以创建特定类型的对象，缺点是 多个实例重复创建方法

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayName = function () {
    alert(this.name);
  };
}
var person = new Person('小明', 13);
console.log(person); //Person {name: "小明", age: 13, sayName: ƒ}
```

#### `Object.create()`

传入一个原型对象，创建一个新对象，使用现有的对象来提供新创建的对象的`__proto__`，实现继承

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  },
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction(); // expected output: "My name is Matthew. Am I human? true"
```

#### 动态原型

优点 可以判断某个方法是否有效，来决定是否需要初始化原型，if 只会在仅在碰到第一个实例调用方法
时会执行，此后所有实例共享此方法，需要注意的是，不能重新原型对象。

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  if (typeof this.sayName != 'function') {
    Person.prototype.sayName = function () {
      alert(this.name);
    };
  }
}
var person = new Person('小红', 15);
console.log(person); //Person {name: "小红", age: 15} 动态创建sayName: ƒ ()
```

### **Question:** 整数一组，请把他们分成三份数组，确保每一份的 **和** 尽量相等

`11，42，23，4，5，6 4 5 6 11 23 42 56 78 90`

```js
/**
 * @param array 原始数组
 * @param count 要分成的份数
 **/
const func = (array, count) => {
  // 将原始数组排序， 从大到小
  array.sort((a, b) => b - a);

  // 每一个大组的 平均值
  let avg = array.reduce((a, b) => a + b) / count;

  //从大到小求和，取最接近平均值的一组，放入 一个 二维数组
  let res_arr = [];
  let current = 0;

  for (let i = 0; i < count - 1; i++) {
    // "&& i" 跳过第一轮循环。
    // 判断第一轮循环结束后，current 加上 array 最后一个元素（最小的一个数）的一半， 是否比每组的 和  小

    // 注意：  因为 下文的 forEach 循环，回遍历的最后一个元素，说明最后一个元素加current 会大于 avg， 因此取最后一个元素的一半相加进行判断

    // 如果小，则： 将倒数第二个元素（如果存在的话） push 进入 结果二维数组的 上一个循环产生结果（数组元素）中。
    // 因为，为了确保分组的和尽量相等，（若加上最后一个元素，已经超过avg）
    if (current + array[array.length - 1] / 2 < avg && i) {
      array.pop();
      // i >= 1
      res_arr[i - 1].push(array[array.length - 1]);
    }

    // 将上一轮 current 重新归零
    current = 0;
    res_arr[i] = [];

    // 遍历原始数组，1.从头到尾，对每个元素求和。 2. 删除 已经加过的元素 3. 将各个元素，push至，res_arr第i个数组元素。
    // note: 第二个参数是index
    array.forEach((item, index) => {
      current += item;
      array.splice(index, 1);
      res_arr[i].push(item);

      // 遍历想加的过程中，如果 和 大于 平均值avg，（说明，一个分组以完成，且多加了一个元素），
      // 从res_arr[i]删除多加的元素。当然原始数组中还需要把多删除的元素，加回去（并且是数组首位）。
      if (current > avg) {
        current -= item;
        array.splice(index, 0, item);
        res_arr[i].pop();
      }
    });
  }

  // 因为，以上主循环结束点是 count-1，所以，array中的剩余元素就是： 分组的最后一组（减少一轮计算，提高性能）。
  // 将剩余的数组，作为res_arr的最后一个元素， push
  res_arr[count - 1] = array;

  return res_arr;
};

//test
console.log(func([11, 42, 23, 4, 5, 6, 4, 5, 6, 11, 23, 42, 56, 78, 90], 3));
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
