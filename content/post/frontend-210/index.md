---
title: '毎日のフロントエンド 210'
date: 2022-04-20T21:14:59+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `:link`、`:visited`、`:hover`、`:active`的执行顺序

- `:link`(未访问链接)，`:visited`(已访问的链接)，`:hover`(鼠标悬停)，`:active`(鼠标按下)
- `a`标签伪类 正确的顺序 是：**lvha(:link-->:visited-->:hover-->:active)**

  - _同等优先权的样式，写在后边的会覆盖前边_
  - a 标签的伪类结合了不同的动作顺序，动作的触发顺序决定了伪类的顺序必须按 `lvha` 来写
  - 前 2 者两种状态是*常态*，而后 2 者是*即时状态*，当即时状态触发时，要覆盖常态，所以**2 个即时状态要放在后边**
  - 在**常态**下：如果 a 标签被访问过后，就要呈现被访问过的状态，所以 visited 要放在 link 后边；
  - 鼠标按下时，伴随着悬停的 a 标签上，所以要想 active 覆盖 hover，就**必须把 active 放后边**；

- `:link` 和 `a` 的样式有可能会冲突
- **当 标签的 href 属性为空的时候，`:link` 样式不会生效；当 标签的 href 属性不为空的时候，`:link` 样式才会生效，这时候，如果 标签正常样式 和 a：link 冲突了的话，以写在后面的那个为准**

## Tips

### TypeScript

- Deep Partial

```ts
type DeepPartial<Thing> = Thing extends Function
  ? Thing
  : Thing extends Array<infer InferredArrayMember>
  ? DeepPartialArray<InferredArrayMember>
  : Thing extends object
  ? DeepPartialObject<Thing>
  : Thing | undefined;

interface DeepPartialArray<Thing> extends Array<DeepPartial<Thing>> {}

type DeepPartialObject<Thing> = {
  [Key in keyof Thing]?: DeepPartial<Thing[Key]>;
};

interface Post {
  id: string;
  comments: { value: string }[];
  meta: {
    name: string;
    description: string;
  };
}

// Partial<> is one level
const post: Partial<Post> = {
  id: '001',
  meta: {},
};

// DeepPartial makes all level properties partial
const post: DeepPartial<Post> = {};
```

[Playground Link](https://www.typescriptlang.org/play?jsx=0#code/C4TwDgpgBAIhFgAoEMBOwCWyA2AeAKgBYYB2A5gHxQC8URpZUEAHsBCQCYDOUAYgK4kAxpgD2JALAAoKFAD8dYuWmyAXIoZNW7blACCqVMhC5SAMwiooASRIXDEDgaMgAshAC2AI0sUV82HgkNEwcZ2NcW3tUR3C3Tx9UPxkodXpyLTZOHlEvACsIEX8FOAQUdCxsAHl8wuACJUp-NMaoAB8oQQ4IM1JHAG5paVI2VDNkIWhS4Iqwwwj0ykydHjjcafLQvEWKKgBvAF8hqVBIQLKQypqCkQaGKlo9-wBtAGkIEChSKABrD9EzBpyABdOTqDaXHB3chvD7AiiDKQHRHDEijcaTKCIURcYBQJ4pDAcdS41AMRGyISiDwedjALjqPZQABuOH4EBJwDJGQOz2BFKgtOAyEZ-lkJGQtM53LIAtk3S4QjJYDEJGl5P8yOkRyk0gA9HqsZC8FQMDkSNBsBBmRBsNIqSRcVAwDjgFULepNpVcNjcQ98f4ieoAOQABlDAEZgwAafxCkX4g6xpEoqQG84zLaC5B-Hg4bBQK02gtgVCiSAVCA8MDG44Op0u3H4ADuonBQS9UN9wH9BNkQYAROGAEwD5OyeOqQ7jqBUml0hl87X9IA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [Inferring Within Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types)
