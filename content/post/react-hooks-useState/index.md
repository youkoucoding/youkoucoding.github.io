---
title: 'React HooksのUseState'
date: 2021-09-29T00:28:55+09:00
description: Reactコアhooksの解説
image: hooks-cover.jpg
categories:
  - React Hooks
---

# useState: 让函数组建具有维持状态的能力

## Example

```javascript
import React, { useState } from 'react';

const Example = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
};
```

## Usage

1. `useState(initialState)` 参数 `initialState` 是创建 state 的初始值， 它可以是**任意类型**

2. `useState()`的返回值是一个有着两个元素的数组。 第一个数组元素用来读取 `state` 的值， 第二个则是用来设置这个 state 的值的函数。 `state`为只读变量， 且必须通过`setCount`来设置 `state`

3. 如果要创建多个 state， 需要多次调用`useState`

   ```js
   const [age, setAge] = useState(99);
   const [fruit, setFruit] = useState('banana');
   const [todos, setTodos] = useState([{ text: 'react hooks' }]);
   ```

## Principles

1. `useState`中的 set 与 `class` 组件中 `setState` 类似， 但是 class 组件中的 state 只有一个，因此在类组件中，一般采用**对象作为一个 state**； `useState`可以多次创建，更加语义化。

2. `state`中**永远不要保存可以通过计算得到的值。**

   - **从 props 传递过来的值**。 sometimes props 传递过来的值无法直接使用，而是需要通过一定的计算后再在 UI 上展示， 比如排序等。 此时， 在需要使用此数据的时候都重新排序，或者， 利用缓存机制， 而不是将结果直接置入`state`
   - **从`URL`中读取的值**。 例如，有时需要读取 url 中的参数，作为组件的一部分状态。 此时， 应在需要的时候读取，而不是存入 state
   - **`cookies` `localStorage`中读取的值**，应每次都去读取。
