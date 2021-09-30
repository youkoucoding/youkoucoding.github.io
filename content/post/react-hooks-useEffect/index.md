---
title: 'React HooksのUseEffect'
date: 2021-09-30T14:24:15+09:00
description: Reactコアhooksの解説, useEffect, Dependencies, eslint
image: hooks-cover.jpg
categories:
  - React Hooks
---

# useState: 执行副作用

### 副作用的定义

通常，副作用是指一段和当前执行结果无关的代码。比如说要修改函数外部的某个变凉，或者发起一个请求等。 在函数组建本次执行中，useEffect 中代码的执行是不影响渲染出来的 UI。

Side effects are all the operations that affect your componet and can't be done during rendering. Things like fetching data, subscriptions or manually changing the DOM are all examples of side effects.

### Usage

```jsx
useEffect(callback, dependencies);
```

- `callback`, 要执行的回调函数；
- `dependencies` 可选的依赖项数组。可选， 如果没有依赖项，callback 在函数组建**每次**执行完成之后都执行;如果依赖项是空数组，则只在第一渲染时实行回调函数； 如果指定依赖项，则在依赖项变化时才执行 callback

- **`useEffect`** 是每次组件 render 结束后，判断依赖项并执行。

1. **No dependencies array:** the function runs after **every render**
2. **Empty dependencies array:** the function runs only after the **first render** _(use this for the stuff it's doing will never get stale)_
3. **Dependencies array with some values:** the function runs only if any of those values change
4. 此外，`useEffect` 允许**返回一个函数**，用与在组件销毁时，做一些清理操作，防止内存泄漏。 例如，移除监听事件。 类似于 类组件中的 `componentWillUnmount`

---

```jsx
function MyComponent() {
  React.useEffect(() => {
    // I need to do this just once, after the first render
  }, []);
  React.useEffect(() => {
    // But this other thing needs to be done after every render
  });
  return ...
}
```

---

```jsx
import React, { useState, useEffect } from 'react';

function BlogView({ id }) {
  // local state to preserve the blog content
  const [blog, setBlog] = useState(null);

  useEffect(() => {
    // useEffect 的 callback 要避免直接使用 async， 需要进行封装
    const doAsynv = async () => {
      // 当 ID 发生变化时，将当前博客清除， 以保持内容的一致性
      setBlog(null);

      // 发起请求，获取数据
      const res = await fetch('/blog-content/${id}');
      // 将数据存入 state
      setBlog(await res.text);
    };
    doAsync();
  }, [id]);

  const isLoading = !blog;
  return <div>{isLoading ? 'loading...' : blog}</div>;
}
```

> **Attention**: If you use the dependencies array, make sure it includes all values from the component scope (such as props and state) that change over time and that are used by the effect. If you've forgotten a value or thinked that you don't need it in the array, you will produce bugs, because your code will reference stale values from previous renders.

## Something More about useEffect

- 使用多个 `useEffect`, 可以实现关注点分离，即将不相关的逻辑分离到不同的 `useEffect` 中
- 函数适合放在 effect 内部还是外部？

  - 推荐的方案:函数在 `useEffect` 内部, 如下：

    ```jsx
    function ProductPage({ productId }) {
      const [product, setProduct] = useState(null);

      useEffect(() => {
        // 把这个函数移动到 effect 内部后，我们可以清楚地看到它用到的值。
        async function fetchProduct() {
          const response = await fetch('http://myapi/product/' + productId);
          const json = await response.json();
          setProduct(json);
        }

        fetchProduct();
      }, [productId]); // ✅ 有效，只用到了 productId
    }
    ```

  - 如果无法 把一个函数移动到 effect 内部，还有一些其他办法：

    1. 尝试把那个函数移动到组件之外
    2. 如果所调用的方法是一个纯计算，并且可以在渲染时调用，可以在 `useEffect` 之外调用， 让 useEffect 依赖它的返回值
    3. `useCallback`

## Dependencies of Hooks

Hooks 中依赖项的工作机制：

Hooks 提供了监听某个数据变化的能力，这个变化可能会触发组件的刷新， 创建副作用， 更新缓存等。要监听变化的数据，就是指定 hooks 的依赖项。

指定依赖项的注意点：

1. 依赖项中指定的变量，一定要在 回调函数 中用到；
2. 依赖项一般是一个**常量数组**， 而不是一个变量；
3. `React`会使用 _浅比较_ 来对比依赖项是否发生变化，所以要特别注意数组或者说对象类型（各种引用类型）。 例如： 如果是每次创建一个新的饮用类型，即使值和原来比没有变化， 也会被认为是依赖项发生了变化。
   ```jsx
   const Todo = () => {
   // 这里的todos是在函数内部创建的， 实际上每次运行都会产生新的数组。
   // 在依赖比较时， 被认为是发生了新的变化。
     const todos = [{ text: 'learn something' }];
     useEffect(()={
       console.log("todos has changed~");
     },[todos])
   };
   ```
4. **React 会确保 setState 函数的标识是稳定的，并且不会在组件重新渲染时发生变化**。这就是为什么可以安全地从 `useEffect` 或 `useCallback` 的依赖列表中省略 `setState`。(React guarantees that setState function identity is stable and won’t change on re-renders. This is why it’s safe to omit from the useEffect or useCallback dependency list.)

## Hooks 使用规则

1. **只能在函数组建的顶级作用域中使用**，不能在循环，条件判断，或者嵌套函数中执行，必须在顶层

   - Hooks 在组件的多次渲染之间，必须按顺序执行。即：
     1. 所有 Hooks 都必须被执行到。（不能将 hooks 放在可能的 return 之后）
     2. Hooks 必须按顺序执行

2. **只能在函数组件或者其他 Hooks 中使用**

   - 如果必须在类组件使用，可以： **利用高阶组件模式，将 Hooks 封装成高阶组件**, 例程如下：

     ```jsx
     import React from 'react';
     import { useWindowSize } from './hooks/useWindowSize';

     export const withWindowSize = (Component) => {
       return (props) => {
         const windowSize = useWindowSize();
         return <Component windowSize={windowSize} {...props} />;
       };
     };
     ```

     ```jsx
     // 使用以上高阶组件
     import React from 'react';
     import { withWindowSize } from './withWindowSize';

     class MyComp {
       render() {
         const windowSize = this.props;
       }
     }

     // 通过windowSize 高阶组件， 给MyComp 添加 windowSize 属性
     export default withWindowSize(MyComp);
     ```

> **`eslint-plugin-react-hooks`** 专门用来检查 hooks 是否被正确使用 [eslint-plugin-react-hooks - npm](https://www.npmjs.com/package/eslint-plugin-react-hooks)

## Reference

[Understanding React's useEffect Hook - DEV Community](https://dev.to/francodalessio/understanding-react-s-useeffect-hook-lbg#:~:text=The%20useEffect%20Hook,-Plain%20and%20simple&text=Side%20effects%20are%20all%20the,likely%20done%20in%20the%20past.)

[javascript 浅比较和深比较 - 掘金](https://juejin.cn/post/6844904056876433415)

[React Hook(useEffect) | Zoeice](http://zoeice.com/react-hook-useEffect/)
