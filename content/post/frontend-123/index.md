---
title: '毎日のフロントエンド  123'
date: 2022-01-17T11:59:49+09:00
description: frontend 每日一练
image: frontend-123-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十三日

## HTML

### **Question:** 用 canvas 画出一个矩形

```html
<canvas id="canvas" width="300" height="300"></canvas>

<script>
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');
    ctx.fillStyle = 'rgb(200,0,0)';
    ctx.fillRect(10, 10, 60, 30);
  }
</script>
```

## CSS

### **Question:** zoom 作用

> zoom 只是 IE 浏览器独有，现在，**除了 FireFox**，其他，尤其 Chrome 和移动端浏览器已经很好支持 zoom 属性
> zoom 依旧不是正式的属性。与之对应的 transform 的 scale 属性是正式的 CSS 属性

- zoom 主要的作用是用于元素或者页面的缩放；transform 的 scale 也有同样的作用，两者有如下的区别:
  - `zoom` 的缩放点在元素左上角。在 Chrome 下，文字如果缩小后小于 12px 的情况仍然会显示 12px。并且 zoom 缩放会影响元素实际的位置，这样就会造成页面的重排和重绘，在性能上会有影响
  - `transform` 的 `scale` 缩放点在元素中心。缩放会对文字有影响，可以利用此属性实现 Chrome 下小于 12px 的字体。但是 transform 缩放后不会改变元素的位置，即如果元素原来是 200px 宽，缩小 50% 后虽然看上去只有 100px 宽了，但是仍然占 200px

## JavaScript

### **Question:** 分析`('b' + 'a' + +'a' + 'a').toLowerCase()`返回的结果

Result: `banana`

`+` 是一元操作符，操作数是 `'a'` => `NaN`

- `'+'`号运算符作为一元运算符时，Expression 将进行`ToNumber()`操作

| argument 类型 | 返回值                                                                               |
| ------------- | ------------------------------------------------------------------------------------ |
| Undefined     | NaN                                                                                  |
| Null          | 0                                                                                    |
| Boolean       | true return 1; false return 0;                                                       |
| Number        | value                                                                                |
| String        | 若字符串为纯数字时返回转换后的数字；非纯数字(包含其他类型字符，无论其位置)均返回 NaN |
| Symbol        | 抛出 TypeError 异常                                                                  |
| Object        | 1.先进行 ToPrimitive(argument, hint Number)得到 rs； 2.然后返回 ToNumber(rs)的结果。 |

## Clean Code JavaScript

### **Classes**

#### 1. Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Bad:**

```javascript
const Animal = function (age) {
  if (!(this instanceof Animal)) {
    throw new Error('Instantiate Animal with `new`');
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function (age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error('Instantiate Mammal with `new`');
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function (age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error('Instantiate Human with `new`');
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Good:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

#### 2. Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, use method chaining and take a look at how clean your code will be. In your class functions, simply return `this` at the end of every function, and you can chain further class methods onto it.

**Bad:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car('Ford', 'F-150', 'red');
car.setColor('pink');
car.save();
```

**Good:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car('Ford', 'F-150', 'red').setColor('pink').save();
```

#### 3. Prefer composition over inheritance

As stated famously in Design Patterns, you should prefer composition over inheritance where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.
This is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
   relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
   (Change the caloric expenditure of all animals when they move).

4. Your inheritance represents an "is-a" relationship and not a "has-a"
   relationship (Human->Animal vs. User->UserDetails).
5. You can reuse code from the base classes (Humans can move like all animals).
6. You want to make global changes to derived classes by changing a base class.
   (Change the caloric expenditure of all animals when they move).

**Bad:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Good:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)
