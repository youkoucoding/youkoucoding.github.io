---
title: '毎日のフロントエンド 213'
date: 2022-04-23T20:15:50+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## Tips

### TypeScript

- `declare global` is super useful too for when you want to allow types to cross module boundaries

```ts
declare global {
  interface GlobalReducerEvent {}
}

export type GlobalReducer<TState> = (
  state: TState,
  event: {
    [EventType in keyof GlobalReducerEvent]: {
      type: EventType;
    } & GlobalReducerEvent[EventType];
  }[keyof GlobalReducerEvent]
) => TState;

export const todosReducer: GlobalReducer<{ todos: { id: string }[] }> = (state, event) => {
  return state;
};
```
