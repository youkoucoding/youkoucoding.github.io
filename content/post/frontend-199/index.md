---
title: '毎日のフロントエンド 199'
date: 2022-04-05T22:53:47+09:00
date: 2022-04-07T21:48:28+09:00
description: frontend 每日一练
categories:
  - HTMl
  - CSS
  - TypeScript Tips
---

## HTML

### 在页面中插入 TAB 符号（制表符）

```html
<pre>
    &#x0009;test test
</pre>
```

- `<pre>` 元素表示预定义格式文本。在该元素中的文本通常按照原文件中的编排，以等宽字体的形式展现出来，文本中的空白符（比如空格和换行符）都会显示出来。(紧跟在 `<pre>` 开始标签后的换行符也会被省略)

## CSS

### Border-radius 50% vs 100%

#### border-radius 的工作原理

> 如果所有圆角的半径都被设置成了 100%，浏览器会根据图形的实际情况做一些计算，保证圆角能够刚好适应图形。

- W3C 对于重合曲线有这样的规范：
  - 如果两个相邻的角的半径和超过了对应的盒子的边的长度，那么浏览器要重新计算保证它们不会重合。
- 一个正方形：如果左上角的圆角半径被设置成了 100%，那么圆角就会从这个方形左下角跨到右上角，相当于把圆角半径设置成为 150px（也就是方形的大小）。如果同时把右上角的圆角半径也设置成为 100%，则两个相邻圆角合起来就有 200%。这种情况自然是不允许出现的，所以浏览器就会重新就算，匀出空间给右边的圆角，同时缩放两个圆角的半径直到它们可以刚好符合这个方形，所以半径就变成了 50%
- `Border-radius:100%`浏览器会对其他的圆角应用相同的计算，计算的结果是每个圆角的半径变成了 50%，所以成了一个圆形

---

- `Border-radius:50%`: 建议使用 `border-radius:50%`来避免浏览器进行不必要的计算

## TypeScript Tips

### Tip 1

- Derive a union type from an object
- 从一个对象中，派生出一个联合类型

```ts
const fruitCounts = {
  apple: 1,
  peach: 2,
  banana: 3,
};

// 从以上对象，得到一下类型
type SingleFruitCount = { apple: number } | { banana: number } | { peach: 3 };

// 应用方式，如下：
const singleFruitCount: SingleFruitCount = {
  banana: 20,
};
```

- solution

```ts
// mapping type
type FruitCounts = typeof fruitCounts;

type SingleFruitCount = {
  [K in keyof FruitCounts]: {
    [K2 in K]: number;
  };
}[keyof FruitCounts];
```

- [Playground](https://www.typescriptlang.org/play?#code/MYewdgzgLgBAZgJwK4EsoGERLFCMC8MA3gLABQMMAhgA40A2ApgFwwCMANOZTY1cAAtWAJi4UYAIyphpVVgGYxAXwDc5clACevGADFkaTNlwEYW3iDjwDGLDggqY6sucYwAyijABzJvtS2xqak4gDaANIwXjAA1oyalno2RvYAuqwhlJQRwlFgMOHpMGBIALYSjAhq4qrkSqFxCVb+hna4qdXOoJCwEF6+jC2BOKyePn7JbcHckrIyIgAMyipAA)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Unicode ASCII List -- EndMemo](http://www.endmemo.com/unicode/ascii.php)

[`<pre>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre)

[Fluent 2014: Lea Verou, "The Humble Border-Radius" - YouTube](https://www.youtube.com/watch?v=JSaMl2OKjfQ)
