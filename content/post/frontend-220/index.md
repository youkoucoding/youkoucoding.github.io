---
title: '毎日のフロントエンド 220'
date: 2022-04-30T20:39:00+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `CSS`表达式 `attr()`

- CSS 表达式 attr() 用来获取选择到的元素的某一 HTML 属性值，并用于其样式。它也可以用于伪元素，属性值采用伪元素所依附的元素。

- 语法: `attr( attribute-name <type-or-unit>? [, <fallback> ]? )`

```html
<p data-foo="hello">world</p>
```

```css
p:before {
  content: attr(data-foo) ' ';
}
```

- result: `hello world`

## TIps

### TypeScript

- Move some complex "Extract" logic to a generic slot, meaning it only gets calculated once.

```ts
export type Obj = {
  a: 'FOO';
  a2: 'a2';
  a3: 'a3';
  b: 'b';
  b1: 'b1';
  b2: 'b2';
};

type ValuesOfKeysStartingWithA<Obj> = {
  [K in Extract<keyof Obj, `a${string}`>]: Obj[K];
}[Extract<keyof Obj, `a${string}`>];

type NewUnion = ValuesOfKeysStartingWithA<Obj>;
```

- Solution

```ts
export type Obj = {
  a: 'FOO';
  a2: 'a2';
  a3: 'a3';
  b: 'b';
  b1: 'b1';
  b2: 'b2';
};

type ValuesOfKeysStartingWithA<
  Obj,
  _ExtractedKeys extends keyof Obj = Extract<keyof Obj, `a${string}`>
> = {
  [K in _ExtractedKeys]: Obj[K];
}[_ExtractedKeys];

type NewUnion = ValuesOfKeysStartingWithA<Obj>;
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0#code/KYDwDg9gTgLgBDAnmYcDyAjAVnAvHAbwFgAoOOAQwC44ByAMTTVoG5TyKAmG2r19ygGYeFQfzJwMPDOPIYAjNPmzJ3Ohk7iAvmxKkkKOADUKAGwCuwAM5oAZgGlgiKwGUYFWAEsAdgHMA6p4wABYAggA8AphYADQCAPoAoiAwUBQAxjDAACaOznCgWd7ZVnAA1k4QtujYeHDJqRkw4RWIVTWxcAAGFAAkBFapPr5aXQB8pGN1xBIA2vZwPnBJKWmZOXlWALo00fNbulqzK43ruU7buvrIqABywADuAKrenhDedSYW1nabbh4wYaBEIRaJjFhAA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [Polyfill CSS attr()新语法](https://www.zhangxinxu.com/wordpress/2020/10/css-attr-polyfill/)
