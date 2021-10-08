---
title: 'JavaScript Tips and Tricks'
date: 2021-10-08T00:47:26+09:00
description: The Collection of JS の 豆知識
image: tip-trick-js.jpg
categories:
  - Javascript
---

## 1. Quick `console.log()`

Get rid of writing console.log again and again and make it shorter by using the following code snippet.

```js
let clog = console.log.bind(document);

clog('This will be printed in console.');

clog(123); // 123
```

## 2. Sprend Operator

The spread operator is a new addition t othe set of operators in `ES6`. It takes an iterable object(e.g array) and expands it into individual elements. Below is an example code of spread operator usage.

```js
// Spread Operator

// Array COncatination

let num_one = [1, 2, 3];
let num_two = [4, 5, 6];
let concat = [...num_one, ...num_two];

console.log(concat); // [1,2,3,4,5,6]

// Copying An Array

let alpha = ['a', 'b', 'c'];
let alpha_copy = [...alpha];

console.log(alpha_copy); // ['a', 'b', 'c']

// Array literals
let data = [100, 200];
let literal = [...data, 300, 400, 500];

console.log(literal); // [100, 200,300,400,500,600]
```

## 3. Truncating any Array

Do you know **the length method** not only show you the size of a String but also truncates your array in any size? Check the below code example:

```js
// Truncating an array

let arr = [100, 200, 300, 400, 500, 600];

// make size 3

arr.length = 3;
console.log(arr); // [100, 200, 300]

arr.length = 0;
console.log(arr); // []
```

## 4. Smart Replacing

This tip will save you time by using loops to replace words in long string data. We will use `repace()` method in JavaScript which takes two parameters one is the **regex** of a word that you want to replace and the second is the new word.

This comes in handy when you working on big Text data and you want to replace specific words with some new words. Check out the below code example for better understanding.

```js
// Smart Replacing

var str = 'This is potato and potato';

console.log(str.replace(/pot/, 'tom')); // This is tomato and potato

console.log(str.replace(/pot/g, 'tom')); // This is tomato and tomato
```

## 5. Numerical Separator

This simple tip will increase the readability of your big number of data in JavaScript. We will use `"_"` as a numberical sepatator. Check out the below code example.

```js
let data1 = 100300400;

let data2 = 100_300_400;

console.log(data2); //100300400;

console.log(data1 === data2); // true
```

## 6. Quick Power Calculation

You probably use `Math.pow()` method to calculate the power of any number. This trick will calculate the power in the quick form by using regular math ways.

```js
// Normal way

console.log(Math.pow(2, 3)); // 8

// Quick way
console.log(2 ** 3); // 8
```

## 7. Rest Parameter

Rest Parameter syntax is used to handle the infinite number of parameters in the function definition. Below example code will clear you understanding of how useful is rest of parameter is:

```js
function sum(...num) {
  let cal = 0;
  for (let a of num) {
    cal += a;
  }
  console.log(cal);
}

sum(2, 3, 4, 5, 6, 7); // 27
```

## 8. Reverse Any String

This trick will reverse any String datra without using any loop. Take a look at the below example code.

```js
// Reverse any string

const reverse = (string) => string.split('').reverse().join('');

console.log(reverse('Typescript')); // tpircsepyT
```

## 9. Destructuring

Destructuring is a JavaScript expression that allows to unpact values from arrays and bind them to variables.

```js
// Destructing
// Normal way
function fun() {
  return [2, 4, 6];
}
let d = fun();
let x = d[0];
let y = d[1];
let z = d[2];
console.log(x, y, z); // 2  4  6
// destruting way
let [x, y, z] = fun();
console.log(x, y, z); // 2  4  6
// Destructing Swaping
a = 4;
b = (2)[(a, b)] = [b, a];
console.log(a, b); // 4 2
```

## Continuing...
