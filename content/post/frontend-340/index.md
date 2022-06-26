---
title: '毎日のフロントエンド 340~341'
date: 2022-06-26T23:42:54+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
---

## HTML

### input 上传图片怎样触发默认拍照

- 使用 `capture` 属性，capture 的值可以是：
  - `camera` 打开摄像头
  - `user` 打开前置摄像头
  - `environment` 打开后置摄像头以上几个属性都不能保证设备会按照设置的一样打开前置或后置摄像头，如果设备不支持的话，它会使用默认的调用摄像头的行为。
  - `camcorder` 打开录像
  - `microphone` 打开录音

## CSS

### css 的加载会阻塞 DOM 树解析和渲染?

- css 的加载不会阻止 DOM 树的解析
- css 的加载会阻止 DOM 树的渲染，因为 css 的下载完成后解析成 CSSOM 与 DOM 生成渲染树后，页面才会渲染，绘制出来

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [HTML Media Capture Examples](http://anssiko.github.io/html-media-capture/)
