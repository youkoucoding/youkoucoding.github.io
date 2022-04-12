---
title: '毎日のフロントエンド 204'
date: 2022-04-12T22:27:42+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### TypeScript

- Type Helpers - PropsFrom Helper - Get the props' type from the other Component

```ts
import React from 'react';

// function component
const MyComponent = (props: { enabled: boolean }) => {
  return null;
};

//class component
class MyOtherComponent extends React.Component<{
  enabled: boolean;
  data: string;
}> {}

// implement the PropsFrom
type PropsFrom<TComponent> = TComponent extends React.FC<infer Props>
  ? Props
  : TComponent extends React.Component<infer Props>
  ? Props
  : never;

const props: PropsFrom<typeof MyComponent> = {
  enabled: true,
};
```

[Playground Link](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgcilQ3wG4BYAKCoHoa5MBXAOw2AmbjV0maWZhVuzAM7wAsgE8Awjw794AXjgAKMDjAiAXHADecfigBGAGyQATHUYgQzKZgBo45lDBRaxUYMwDmcAL4AlHCKAHx6VHBwRDCMUJzMjCYmFJT+qbQ0aCYoIiJccnwCQjl5cFIA8jAAFkhQsuDyAgYAHjD85vnI6DAAdA28CgA8upEGzMZmlnDWtqiOY0TacJ7ePg5U-uG6-lSZcKBgZiAKcDVIcAAKGiIAYjggVDCSYBfXEJr3uEMAKgNNMHCyj+hVOSDaHS6xD6t2kQ28mDqVxuoTGAH5kR8RGMdCDGkV4OD2sxOohof1QQJ4cxEVBMZpUZQohj3pocXA+AA3OoZSjCMRwdRYnSsu4PIbPV4QTDlGSUwEhCJM8aTCw6GBQRhIDbKlxuLQAIgAjAAmADMBs2vP58GyuREopFNy+IAlLyQ0tlVVq9XlQNGysMpjVmBQJhE2sWSG0BqWBp1-iAA)

## Reference

[TypeScript: Documentation - Conditional Types - infer keyword](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types)

[ TypeScript with React | React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/docs/basic/setup/)

[精读《Typescript infer 关键字》](https://juejin.cn/post/6999441997236797470)
