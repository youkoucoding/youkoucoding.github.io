---
title: '毎日のフロントエンド 242'
date: 2022-05-10T23:01:30+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### `useState`

#### `state`结构

- useState 接收一个 initialState，同时返回一个 state 以及更新 state 的函数
- 不推荐将数据集中于一个 useState

#### 避免重复计算

- 如果 initialState 为函数，则 useState 在初始化时会*立刻执行该函数*和获取函数的返回值，在没有任何返回值得情况下为 undefined
- 需要注意的是每次组件 re-render 都会导致 useState 中的函数重新计算，这里可以使用闭包函数来解决问题, 例如：

优化前：

```js
const loop = () => {
  console.log('calc!');
  let res = 0;
  for (let i = 0, len = 1000; i < len; i++) {
    res += i;
  }
  return res;
};

const [value, setValue] = useState(loop());
```

优化后：

```js
// 优化后只有组件初始化时才会执行一遍loop函数
const App = () => {
  const [value, setValue] = useState(() => {
    return loop();
  });
};
```

#### 更新 `state`

- 在某些情况下如果需要从上一个 state 来计算当前的 state，可能会想到使用下面优化前的方法。但是，需要注意的一点是在某些闭包的场景下面 count 可能不是最新的，这样会导致计算错误。这里推荐使用官方给出的方法。

优化前：

```js
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>+</button>;
}
```

优化后：

```js
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((prevCount) => prevCount + 1)}>+</button>;
}
```

## Reference

- [ Hooks 实践（一）](https://www.linxiangjun.com/react-hooks-best-practices-1.html)
