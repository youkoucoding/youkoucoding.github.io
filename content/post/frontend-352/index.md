---
title: '毎日のフロントエンド 352~356'
date: 2022-07-03T17:38:51+09:00
description: frontend 每日一练
categories:
  - JavaScript
  - Tips
---

## HTML

### `noscript`标签

- `noscript` 标签用于当浏览器不支持 JS 的时候在页面上显示一些提示内容

  - 也有一些缺点，比如如果是防火墙而不是浏览器禁用了 JS， JS 执行不了，noscript 的内容也不会显示

## Tips

### `async/await` 是把双刃剑

```js
(async () => {
  const pizzaData = await getPizzaData(); // async call
  const drinkData = await getDrinkData(); // async call
  const chosenPizza = choosePizza(); // sync call
  const chosenDrink = chooseDrink(); // sync call
  await addPizzaToCart(chosenPizza); // async call
  await addDrinkToCart(chosenDrink); // async call
  orderItems(); // async call
})();
```

- 当 pizzaData 与 drinkData 之间没有依赖时，顺序的 await 会最多让执行时间增加一倍的 getPizzaData 函数时间，因为 getPizzaData 与 getDrinkData 应该并行执行。

---

- 正确的做法应该是先同时执行函数，再 await 返回值，这样可以并行执行异步函数：

```js
(async () => {
  const pizzaPromise = selectPizza();
  const drinkPromise = selectDrink();
  await pizzaPromise;
  await drinkPromise;
  orderItems(); // async call
})();
```

- 或者使用 `Promise.all` 可以让代码更可读：

```js
(async () => {
  Promise.all([selectPizza(), selectDrink()]).then(orderItems); // async call
})();
```

#### 理解语法糖

- 首先 async/await 只能实现一部分回调支持的功能，也就是仅能方便应对层层嵌套的场景

```js
a(() => {
  b();
});

c(() => {
  d();
});

//  ====

await a();
await b();
await c();
await d();

// 上述代码的实际效果如下

a(() => {
  b(() => {
    c(() => {
      d();
    });
  });
});
```

- 利用 `Promise.all`:

```js
async function ab() {
  await a();
  b();
}

async function cd() {
  await c();
  d();
}

Promise.all([ab(), cd()]);
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [How to escape async/await hell](https://www.freecodecamp.org/news/avoiding-the-async-await-hell-c77a0fb71c4c)

- [精读《async/await 是把双刃剑》 · Issue #82 · ascoders/weekly](https://github.com/ascoders/weekly/issues/82)

- [weekly/55.精读《async await 是把双刃剑》.md at master · ascoders/weekly](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/55.%E7%B2%BE%E8%AF%BB%E3%80%8Aasync%20await%20%E6%98%AF%E6%8A%8A%E5%8F%8C%E5%88%83%E5%89%91%E3%80%8B.md)
