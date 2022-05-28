---
title: '毎日のフロントエンド 296~297'
date: 2022-05-28T17:24:12+09:00
description: frontend 每日一练
categories:
  - HTML
  - Tips
---

## HTML

### 文字贯穿线

- `<del></del>`

## CSS

### `font-size-adjust`

- `font-size-adjust` CSS 属性定义字体大小应取决于小写字母，而不是大写字母。
  - 在字体较小时，字体的可读性主要由小写字母的大小决定，通过此选项即可进行调整。

### `@font-face`

- `src`
  - 远程字体文件位置的 URL 或者用户计算机上的字体名称， 可以使用 `local` 语法通过名称指定用户的本地计算机上的字体( i.e. src: local('Arial'); )。 如果找不到该字体，将会尝试其他来源，直到找到它。

## Tips

### `setTimeout` 和 `setInterval`

- `setTimeout()` 只执行函数一次, 也就是说当达到设定的时间后就开始运行指定的代码，运行完后就结束了，次数是一次
- `setInterval()` 是循环执行的, 即每达到指定的时间间隔就执行相应的函数或者表达式，只要窗口不关闭或 clearInterval() 调用就会无限循环下去
  - 使用 setInterval 时，某些间隔会被跳过；即使 setInterval 调用的方法报错了，他仍然会继续执行。
  - 无视网络延迟，可能多个定时器会连续执行

### React 代码整洁之道

#### 不冗余

- 避免重复代码段，对 JSX 同理：

```jsx
// Dirty
const MyComponent = () => (
  <div>
    <OtherComponent type='a' className='colorful' foo={123} bar={456} />
    <OtherComponent type='b' className='colorful' foo={123} bar={456} />
  </div>
);

// Clean
const MyOtherComponent = ({ type }) => (
  <OtherComponent type={type} className='colorful' foo={123} bar={456} />
);
const MyComponent = () => (
  <div>
    <MyOtherComponent type='a' />
    <MyOtherComponent type='b' />
  </div>
);
```

#### 可预测、可测试

- [americanexpress/jest-image-snapshot: ✨ Jest matcher for image comparisons. Most commonly used for visual regression testing.](https://github.com/americanexpress/jest-image-snapshot)

#### 自我解释

- 尽可能减少代码中的注释。可以通过让变量名更语义化、只注释复杂、潜在逻辑，来减少注释量，同时也提高了可维护性，毕竟不用总在代码与注释之间同步了

```jsx
// Dirty
const fetchUser = (id) =>
  fetch(buildUri`/users/${id}`) // Get User DTO record from REST API
    .then(convertFormat) // Convert to snakeCase
    .then(validateUser); // Make sure the the user is valid

// Clean
const fetchUser = (id) =>
  fetch(buildUri`/users/${id}`)
    .then(snakeToCamelCase)
    .then(validateUser);
```

#### 斟酌变量名

- 布尔值或者返回值是布尔类型的函数，命名以 is has should 开头

```jsx
// Dirty
const done = current >= goal;
// Clean
const isComplete = current >= goal;
```

- 函数以其效果命名，而不是怎么做的来命名

```jsx
// Dirty
const loadConfigFromServer = () => {
  ...
};
// Clean
const loadConfig = () => {
  ...
};
```

#### 遵循设计模式

- 单一责任原则, 确保每个功能都完整完成一项功能，比如更细粒度的组件拆分，同时也更利于测试。
- 不要把组件的内部依赖强加给使用方。
- lint 规则尽量严格。

#### React 中的实践

- 在 React 使用 defaultProps 代替在代码中动态判断

  - 显然，利用 React 组件的规则，将入参的默认值预先定义好是最高效的。如果在 ts 最严格的 stricts 模式里，依然会报错：变量可能未定义。因为 defaultProps 依然是个约定，而不能通过强类型推导出，目前还没有更优雅的解决思路。

- 渲染与判断逻辑分开
  - 逻辑与渲染分离，便于维护，其次便于测试

#### 解构

```js
// Dirty
const splitLocale = locale.split('-');
const language = splitLocale[0];
const country = splitLocale[1];

// Clean
const [language, country] = locale.split('-');
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [font-size-adjust - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size-adjust)

- [setTimeout 和 setInterval](https://www.cnblogs.com/joe235/p/13402282.html)

- [`@font-face` - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)

- [Clean Code vs. Dirty Code: React Best Practices - American Express Technology](https://americanexpress.io/clean-code-dirty-code/)

- [精读《React 代码整洁之道》.md at master · ascoders/weekly](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/34.%E7%B2%BE%E8%AF%BB%E3%80%8AReact%20%E4%BB%A3%E7%A0%81%E6%95%B4%E6%B4%81%E4%B9%8B%E9%81%93%E3%80%8B.md)

- [americanexpress/jest-image-snapshot: ✨ Jest matcher for image comparisons. Most commonly used for visual regression testing.](https://github.com/americanexpress/jest-image-snapshot)
