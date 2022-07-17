---
title: '毎日のフロントエンド 372~375'
date: 2022-07-17T14:12:46+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### http 请求头中`Referer`

- `referer` 请求头包含了**当前请求页面的来源页面的地址**，即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 Referer 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。

### Chrome DevTools: Easily control typography with the CSS Font Editor

> Availability
> This feature is in Chrome Stable, however it needs enabling from the Experiments Panel. To enable:

> From DevTools, enter Cmd + Shift + P > Show Experiments.
> Type in font in the Experiments Filter box.
> Enable the Font Editor Experiment.

- Inspect some text with DevTools.
- Click on the Font Editor icon within the Styles Pane (marked by two overlapping "A"'s).
- Make some changes within the Font Editor and observe the page updates instantly.

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [Referer - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer)
- [Font Editor - Chrome DevTools - Dev Tips](https://umaar.com/dev-tips/244-font-editor/)
