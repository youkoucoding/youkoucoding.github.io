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

### `useEffect`

> 每次运行都相当于对**当前的状态**存储了一份快照，这就这意味在 useEffect 当中执行 setTimeout 获取到的都不是最新的状态，而是运行时的状态

#### 异步函数 - 注意事项

- 错误的示范

```js
useEffect(async () => {
  const { data, result } = await asyncRequest();
}, []);
```

- useEffect 不建议在回调函数中直接使用 async，这样使用会直接触发报错，通常如果安装了相关 lint 的话会有提示。因为这么使用会导致回调函数相互之间产生竞争状态，而 **Effect 回调函数应该是同步的**

- 推荐的做法

```js
const requestFn = async () => {
  const { data, result } = await asyncRequest();
};

useEffect(() => {
  requestFn();
}, []);
```

- 如果需要在获取异步数据之后更新 state，也可以在 requestFn 函数中处理

```js
const [value, setValue] = useState();

const requestFn = async () => {
  const { data, result } = await asyncRequest();
  setValue(value);
};

useEffect(() => {
  requestFn();
}, []);
```

#### 依赖

- useEffect 在没有任何依赖项时每次都会执行一遍， 如果在它当中改变了 state，那么就会导致死循环， 过程就是执行 useEffect 改变了 state，而改变 state 又导致了重复执行 useEffect

- 错误的写法

```js
const [count, setCount] = useState(0);

useEffect(() => {
  setCount((prevCount) => prevCount++);
});
```

- 正确的写法

```js
const [count, updateCount] = useState(0);
const [num, updateNum] = useState(1);

// 依赖num，每次num改变后才会执行Effect
useEffect(() => {
  updateCount((prevCount) => prevCount + num);
}, [num]);
```

##### 函数作为依赖

- 除了使用 state、props 作为依赖项，函数也是可以直接作为依赖项使用。不过由于函数每次在组件渲染时都会重新执行，所以 Effect 会没必要的重复执行

- 可以使用 useCallback 方法来避免这种情况

```js
// 每次渲染都重复执行Effect
const doSomething = () => {};

useEffect(() => {
  // do somethings
}, [doSomething]);
```

```js
// 使用useCallback
const doSomething = () => useCallback(() => {}, []);

useEffect(() => {
  // do somethings once
}, [doSomething]);
```

- 如果函数依赖任何其他的状态执行，则可以将依赖加入到 useCallback 的依赖数组项中，这样在依赖项改变后函数都会重新执行，Effect 由于依赖了函数，所以 Effect 也会执行

#### 优化

- 正确的使用 Effect 的依赖是非常重要的，每次依赖改变后都会使用 Object.is 方法来比较，如果不同，则执行 Effect
- 可以通过 Memoization 技术来优化，具体来说就是每次状态改变后都会与之前记忆的状态作对比，如果值发生了改变，则返回新的状态，并记忆，如果值没有改变，只是引用变了，则返回记忆的状态。这样 Effect 只在值改变后才执行

### `useLayoutEffect`

- 大多数情况下使用 useEffect 即可，不过当需要在 useEffect 中操作 DOM 时，为了优化渲染效果，可以使用 useLayoutEffect。会在 DOM 更新完成之后再执行，同时可以读取到 DOM 布局并同步触发渲染，之后浏览器才进行绘制

## Reference

- [ Hooks 实践（一）](https://www.linxiangjun.com/react-hooks-best-practices-1.html)
