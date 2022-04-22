---
title: '毎日のフロントエンド 211'
date: 2022-04-21T22:36:12+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `clear`属性取值

- clear CSS 属性指定一个元素是否必须移动(清除浮动后)到在它之前的浮动元素下面。clear 属性适用于浮动和非浮动元素。
  - none
    - 元素不会向下移动清除之前的浮动。
  - left
    - 元素被向下移动用于清除之前的左浮动。
  - right
    - 元素被向下移动用于清除之前的右浮动。
  - both
    - 元素被向下移动用于清除之前的左右浮动。
  - inline-start
    - 该关键字表示该元素向下移动以清除其包含块的起始侧上的浮动。即在某个区域的左侧浮动或右侧浮动。
  - inline-end
    - 该关键字表示该元素向下移动以清除其包含块的末端的浮点，即在某个区域的右侧浮动或左侧浮动。

## Tips

### TypeScript

- LooseAutocomplete which gives us autocomplete while also allowing arbitrary values

```ts
type IconSize = 'sm' | 'xs';

interface IconProps {
  size: IconSize;
}

export const Icon = (props: IconProps) => {
  return <></>;
};

const Comp1 = () => {
  return (
    <>
      <Icon size='xs'></Icon>
      <Icon size='something'></Icon> // arbitrary value right here.
    </>
  );
};
```

- For providing arbitrary values Solution

```ts
// type IconSize = 'sm' | 'xs' | Omit<string, 'xs' | 'sm'>;

type IconSize = LooseAutocomplete<'sm' | 'xs'>;

type LooseAutocomplete<T extends string> = T | Omit<string, T>;

interface IconProps {
  size: iconSize;
}

export const Icon = (props: IconProps) => {
  return <></>;
};

const Comp1 = () => {
  return (
    <>
      <Icon size='xs'></Icon>
      <Icon size='something'></Icon> // arbitrary value right here.
    </>
  );
};
```

[Playground Link](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgcilQ3wFgAoCgeirhgE8wk4BJNCAOwGVgAvZgLwEAziHxwAPgQAew8VIDyIYDAA8wmFGAcA5gBoZcySLEA+ANwUGTVu259BcADIQIwpAEEArjAjtwADZIMEiq+KLyhvgWFFaMzC5unj5+uGBBIaoAKnBI0iEcACbCcBpauqZwQjmKympl2vpwWTGU5NohUJjozGycAAo4YCUA3hRwpQ4AXLacPPyW5AC+seR5kLBwdhqzHFVwABRgQ8IzfRyDEMMAlFWVY+QTRDBeUHuqpqpUrUuLq9vwADCaQAjPsDrcBPdxnBnq89gcYRMPkiJnBVOdJvwBPhZNEvudTKjkZjhA4BAAiYS4YIAC0auQC7gpnyohLgNDgKCgACMVFBufQ4AA3FABLzMLQ6WnwWlIIgAOlRXyJjzg10WvyAA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [clear - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)
