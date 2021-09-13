---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（六）
date: 2021-09-13T17:34:37+09:00
description: 知识点： React 状态提升 Lifting state up
image: liftup-cover.jpg
categories:
  - React
  - TypeScript
---

# What is "Lifting State up" and Why we need it?

Lifting state up is a common pattern that is essential for React developer to know. It helps us avoid more complex pattern for managing our state.

Often there will be a need to share state between different components. The common approach to share state between two components is to move the state to common parent of the two compenont.

我们可以很清楚的明白 React 状态提升主要就是用来处理父组件和子组件的数据传递的；他可以让我们的数据流动的形式是自顶向下单向流动的，所有组件的数据都是来自于他们的父辈组件，也都是由父辈组件来统一存储和修改，再传入子组件当中。

---

> We lift up state to a common ancestor of components that need it, so that they can nall share in the state. This allows us to mor4e easily share state among all of these components that need rely upon it.

- There should be a single "source of truth" for any data that changes in application. Usuall, the state is first added to the component that needs it for rendering. If any other components also need it, you can lift it up to their **closest common ancestor**. Instead of trying to sync thje state between different components, you should rely on **the top-down data flow.**

- Lifting state takes less work to find and isolate bugs. Since any state "lives" in some component and that component alone can change it. Additionally, we can implement any custom logic to reject or transform or validate user input.

- If something can be derived from eother props or state, it probably shouldn't be in the state.
