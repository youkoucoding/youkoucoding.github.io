---
title: '毎日のフロントエンド 216'
date: 2022-04-26T23:16:47+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### TypeScript

- Mapping over a union type can feel tricky to conceptualise.
- **Distributive Conditional Types**

```ts
export type Letters = 'a' | 'b' | 'c';

type RemoveC<TType> = any;

type WowWithoutC = RemoveC<Letters>;
```

- Solution

```ts
// RemoveC - a type helper to remove "c" from a union of letters.

export type Letters = 'a' | 'b' | 'c';

// extends 会将union type 中的每一个类型 进行对比
type RemoveC<TType> = TType extends 'c' ? never : TType;

type WithoutC = RemoveC<Letters>;
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0#code/PTAECUFMFsHsDdIGFQFpQENQBcCeAHSUAC0gBtCAnHWUSmBIgIgGMnQAzS2aTUAVwB2AS1iDQsDqDKRs2SJQDOAOgCwAKA2QAHvliVsOAkQAys+UtABeUAHIMt0AB87AI0cvbLWwG4NGvEIIBkQkAB4AFQjjAD5rUCjjUB15QQATRTtvUAB+UEFIRGoALgTowj9NdUCiAHVhbGJYfmwUGyg4ULCzOQVFGJ8gA)
