---
title: '毎日のフロントエンド 203'
date: 2022-04-11T22:12:08+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### canvas 的`width`与`height`属性的值可不可以带单位

- `width/height`的属性值可以带单位且不会报错，但是无论是 px 还是 em、rem，表现都和 px 一致

## CSS

### `height`和`line-height`

- `line-height`:
  - 在日常用的最多的是让单行文字垂直居中（其实不需要设置 height，一个 line-height 即可）。因为 line-height - font-size 为行距，一般会近似平分到文字的上下两边，使文字看上去垂直居中。如果需要多行文字的垂直居中，还需要加上 `vertical-align: middle;`
  - line-height 可以不设置单位，表示 font-size 的倍数
- `height`
  - 指定了一个元素的高度。默认情况下，这个属性决定的是内容区（ content area）的高度，但是，如果将 box-sizing 设置为 border-box , 这个属性决定的将是边框区域（border area）的高度。

## Tips

### TypeScript

- The `extends` keyword: narrow the value of a generic to enable some autocomplete type of interface

```ts
const getDeepValue = <Obj, FirstKey, SecondKey>(
  obj: Obj,
  firstKey: FirstKey,
  secondKey: SecondKey
) => {
  return {} as any;
};

// to

const obj = {
  foo: { a: true, b: 2 },
  bar: { c: 'cool', d: 2 },
};

const result = getDeepValue(obj, 'bar', 'd');
```

- Solution

```ts
const getDeepValue = <Obj, FirstKey extends keyof Obj, SecondKey extends keyof Obj[FirstKey]>(
  obj: Obj,
  firstKey: FirstKey,
  secondKey: SecondKey
): Obj[FirstKey][SecondKey] => {
  return {} as any;
};
```

[Playground Link](https://www.typescriptlang.org/play?#code/MYewdgzgLgBA5gUygEQQgDgNQIYBsCuCMAvDADwDyARgFYA0MAYgJYBO0A0ggJ4wIAeUBGAAmEGAGseIAGYxq9GAGUEoUV14Cho8VO6z5tANot2UDQF0AfAAoAsACgYMELQBch+o+cy2nHh6m-tx03jAQquAiGh4qatE8jgCUHgomfuY8FkZxUZYkVjAA3mGsSPisYMUAvjDY4thg3ADcjtWtDo5q0C60JMVhMiAgHkV1HlCshAxUHgBMMNWhTjBU2KyjMMAeAOSgILg7DCLzi8vtjpcOAPTXMADuRMCN8EgwUAAWRFDc6EQGZQg+FwsCoCGw+CgzBkwNw3C64B6gOBsFIiBQaCweEINlcih2a1YRxgex2SWaQA)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<canvas>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas#attr-width)

[CSS 深入理解之`line-height`](https://www.bbsmax.com/A/8Bz84NM6zx/)

[line-height - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)

[height - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)
