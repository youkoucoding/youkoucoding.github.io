---
title: '毎日のフロントエンド 202'
date: 2022-04-10T21:21:26+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `<dialog>`

- HTML `<dialog>` 元素表示一个对话框或其他交互式组件，例如一个检查器或者窗口。
- Attributes - 属性
  - `open`: 指示这个对话框是激活的和能互动的。当这个 open 特性没有被设置，对话框不应该显示给用户。(没有它，这个对话框就会隐藏起来，直到你使用 JavaScript 来显示它)
  - `returnValue`: 用来获取 close 时传入的参数
- 方法：
  - `show()` `showModal()`
    - 两个方法相同点都是打开弹窗，即都会给 dialog 元素添加一个 open 属性。
    - 不同点： 唯一区别是`show()`会按照其在 DOM 流中的位置显示 dialog，没有遮罩，而`showModal()`会出现遮罩， 并且自动做了按键监控，即点击 esc 键，弹窗会关闭
    - 大多数情况下，使用便利的 `showModal()` 方法来而不使用 show()方法。
  - `close()`
    - 会关闭弹窗，即会删除 open 属性，并且可以携带一个参数作为额外数据，传入的值可以通过 `dialog.returnValue` 获取。
- 两个事件:
  - `close`: 当 modal 关闭的时候触发
  - `cancel`: 当按下 ESC 关闭模态框的时候触发
  - 在各事件的事件对象 event.target 里，同样可以看到 close()方法传入的参数，即 `event.target.returnValue`
- 一个伪元素:
  - `::backdrop` : 是 dialog 伪元素，用来设置弹窗遮罩的样式，用法如下:

```css
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.4);
}
```

## JavaScript

### js 获取当前的 url 和来源的 url

- 当前 url `window.location.href`

- 来源 url `document.referrer`

## Tips

### TypeScript

- Functional overload used in Conjunction with _Generics_
- 组合函数 - 函数重载结合泛型

```ts
export function compose(...args: any[]) {
  return {} as any;
}

const addOne = (a: number) => {
  return a + 1;
};

const numberToString = (a: number) => {
  return a.toString();
};

const stringToNumber = (a: string) => {
  return parseInt(a);
};

const addOneToString = compose(addOne, numToString, stringToNumber);
```

- Solution

```ts
export function compose<Input, FirstArg>(
  func: (input: Input) => FirstArg
): (input: Input) => FirstArg;

export function compose<Input, FirstArg, SecondArg>(
  func: (input: Input) => FirstArg,
  func2: (input: FirstArg) => SecondArg
): (input: Input) => SecondArg;

export function compose<Input, FirstArg, SecondArg, ThirdArg>(
  func: (input: Input) => FirstArg,
  func2: (input: FirstArg) => SecondArg,
  func3: (input: SecondArg) => ThirdArg
): (input: Input) => ThirdArg;

// 决定组合后，函数参数的顺序
export function compose(...args: any[]) {
  return {} as any;
}

const addOne = (a: number) => {
  return a + 1;
};

const numberToString = (a: number) => {
  return a.toString();
};

const stringToNumber = (a: string) => {
  return parseInt(a);
};

const addOneToString = compose(addOne, numToString, stringToNumber);
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<dialog>` codepen](https://codepen.io/keithjgrant/pen/eyMMVL)

[初探 HTML5.x 新特性《dialog》标签-阿里云开发者社区](https://developer.aliyun.com/article/374584)

[`<dialog>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dialog)

[TypeScript: Documentation - Type Compatibility](https://www.typescriptlang.org/docs/handbook/type-compatibility.html#functions-with-overloads)

[TypeScript: Documentation - Declaration Reference](https://www.typescriptlang.org/docs/handbook/declaration-files/by-example.html#overloaded-functions)
