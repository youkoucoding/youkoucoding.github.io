---
title: '毎日のフロントエンド　49'
date: 2021-11-04T13:26:16+09:00
description: frontend 每日一练
image: frontend-49-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十九日

## HTML

### **Question:** 说说对`target="_blank"`的理解？有啥安全性问题？如何防范

恶意攻击问题

如果网站上有一个使用了 `target="_blank"` 的 `a` 标签链接，一旦用户点击了这个链接打开了新的标签页，如果这个标签页跳转的网站内存在的恶意代码，那么你原本页面的网站可能会被转到一个假的页面。也就是说，当用户回到原本的页面时，看到的可能就是已经被替换过的钓鱼页面了。

防范

在 `<iframe>` 中有一个 `sandbox` 属性，可以使用以下的一些方法来预防链接:

1. `Referrer Policy` 和 `noreferrer`

```html
<a href="https://an.evil.site" target="_blank" rel="noreferrer">
  Enter an "evil" website
</a>
```

2. `noopener`

```html
<a href="https://an.evil.site" target="_blank" rel="noopener">
  Enter an "evil" website
</a>
```

3 `nofollow`

`nofollow` 是 HTML 页面中 a 标签的属性值。这个标签的意义是告诉搜索引擎"不要追踪此网页上的链接或不要追踪此特定链接"

## JavaScript

### **Question:** 写个还剩下多少天过年的倒计时

```js
const day = Math.floor(
  (new Date('2019-12-31 23:59:59:999') - new Date()) / 864e5
); // 210
```

> `1000*60*60*24 or 86400000 or 864e5`

`864e5` is a valid Javascript number that represents the number of miliseconds in 24 hours day.

## One more question

### **Question:** `redux-saga` 和 `mobx` 的比较

#### 状态管理

`redux-sage` 是 `redux` 的一个异步处理的中间件
`mobx` 是数据管理库，和 `redux` 一样

#### 设计思想

`redux-sage` 属于 `flux` 体系， **函数式编程思想**
mobx 不属于 flux 体系，面向对象编程和响应式编程

#### 主要特点

- `redux-sage` 因为是中间件，更关注**异步处理**的，通过 `Generator` 函数来将异步变为同步，使代码可读性高，结构清晰。action 也不是 action creator 而是 `pure action`，
- 在 `Generator` 函数中通过 `call` 或者 `put` 方法直接声明式调用，并自带一些方法，如 `takeEvery`，`takeLast`，`race` 等，控制多个异步操作，让多个异步更简单。

- `mobx` 是更简单更方便更灵活的处理数据。 `Store` 是包含了 `state` 和 `action`。`state` 包装成一个可被观察的对象， `action` 可以直接修改 `state`，之后通过 `Computed values` 将依赖 `state` 的计算属性更新 ，之后触发 `Reactions` 响应依赖 `state` 的变更，输出相应的副作用 ，但不生成新的 `state`

#### 数据可变性

- `redux-sage` 强调 `state` **不可变**，不能直接操作 state，通过 action 和 reducer 在原来的 state 的基础上返回一个新的 state 达到改变 state 的目的。
- `mobx` 直接在方法中更改 state，同时所有使用的 state 都发生变化，不生成新的 state。

#### 写法难易度

- `redux-sage` 比 redux 在 action 和 reducer 上要简单一些。需要用 dispatch 触发 state 的改变，需要 mapStateToProps 订阅 state。
- `mobx` 在非严格模式下不用 action 和 reducer，在严格模式下需要在 action 中修改 state，并且自动触发相关依赖的更新。

#### 使用场景

- `redux-sage` 很好的解决了 redux 关于异步处理时的复杂度和代码冗余的问题，数据流向比较好追踪。但是 redux 的学习成本比 较高，代码比较冗余，不是特别需要状态管理，最好用别的方式代替。
- `mobx` 学习成本低，能快速上手，代码比较简洁。但是可能因为代码编写的原因和数据更新时相对黑盒，导致数据流向不利于追踪。

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[隐藏危险: target = "\_blank" 和 "opener" - 知乎](https://zhuanlan.zhihu.com/p/53132574)
