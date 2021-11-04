---
title: 'Append Item to Array in JavaScript'
date: 2021-11-04T16:52:18+09:00
image: append-item-array-cover.jpg
categories:
  - Javascript
---

# 5 Way to Append Item to Array in JavaScript

Here are 5 ways to add an item to the end of an array.

`push splice length` will mutate the orginal array.

Whereas `concat` and `spread` will not and will instead return a new array.

## Mutative - 3 ways to Append Item to Array

### `push`

```js
const zoo = ['🦊', '🐮'];

zoo.push('🐧');

console.log(zoo); // ['🦊', '🐮', '🐧']
```

Of course, you can push multiple items.

```js
const zoo = ['🦊', '🐮'];

zoo.push('🐧', '🐦', '🐤');

console.log(zoo); // ['🦊', '🐮', '🐧', '🐦', '🐤']

// or

const zoo = ['🦊', '🐮'];
const birds = ['🐧', '🐦', '🐤'];

zoo.push(...birds);

console.log(zoo); // ['🦊', '🐮', '🐧', '🐦', '🐤']
```

### `splice`

| Parameters | Parameter Name | Definition                                                                     |
| ---------- | -------------- | ------------------------------------------------------------------------------ |
| 1          | startIndex     | The index where you want to add/remove item                                    |
| 2          | deleteCount    | The number of items you want to remove                                         |
| 3          | items          | The number you want to add (If you're removing, you can just leave this blank) |

```js
const zoo = ['🦊', '🐮'];

zoo.splice(
  zoo.length, // We want add at the END of our array
  0, // We do NOT want to remove any item
  '🐧',
  '🐦',
  '🐤' // These are the items we want to add
);

console.log(zoo); // ['🦊', '🐮', '🐧', '🐦', '🐤']
```

### `length`

`array.length` returns us the total count of items in the array. That means the length us always one number higher than the last item of out index. So by assigning a value at the length index, it's essentially adding an item to the end of the array.

```js
const zoo = ['🦊', '🐮'];
const length = zoo.length; // 2

zoo[length] = '🐯';

console.log(zoo); // ['🦊', '🐮', '🐯']
```

## Non Mutative - 2 ways to Append Item to Array

The original array will remain untouched and a new array will contain the addition.

### `concat`

This method is meant to merge arrays. So we can use it to add multiple items by passing in an array.

It doesn't just accept arrays as its parameter, it also acceptes value.

```js
const ocean = ['🐙', '🦀'];

const aquarium = ocean.concat('🐡'); // Add a single value
const sushi = ocean.concat('🐡', '🍚'); // Add multiple values

aquarium; // ['🐙', '🦀', '🐡']
sushi; //  ['🐙', '🦀', '🐡', '🍚']

// Original Array Not Affected
ocean; // ['🐙', '🦀']
```

### `spread`

We can uyse the spread syntax to expand each array element into individual elements.
A very popular application is to use spread to crate a copy or merge two separetate arrays. This is similar to the effects of `concat`

```js
const ocean = ['🐙', '🦀'];
const fish = ['🐠', '🐟'];

const aquarium = [...ocean, ...fish];

aquarium; // ['🐙', '🦀', '🐠', '🐟']

// Original Array Not Affected
ocean; //  ['🐙', '🦀']
```

if we don't use `spread`, we will get this: a nested array

```js
const ocean = ['🐙', '🦀'];
const fish = ['🐠', '🐟'];

const aquarium = [ocean, fish];

// [  ['🐙', '🦀'],  ['🐠', '🐟'] ]
```

## To Mutate or NOT

IT Really depends on your use case. When you're working in **Redux** or any state management architecture, then it's all about the **immutability**. So the non-mutatve methods will be the right choices.

Also the idea of immutability is often preferred as it's considered a good practice to **avoid side effects** -- which is the foundation of functional programming and producing pure functions.

## Reference

[5 Way to Append Item to Array in JavaScript | SamanthaMing.com](https://www.samanthaming.com/tidbits/87-5-ways-to-append-item-to-array/)
