---
title: '毎日のフロントエンド 326~330'
date: 2022-06-05T11:45:03+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## HTML

### `<a>`标签的`target`属性

- `_self` 默认。在相同的框架中打开被链接文档
- `-blank` 在新窗口中打开被链接文档
- `_parent` 在父框架集中打开被链接文档
- `_top` 在整个窗口中打开被链接文档

### html5 进行图片压缩上传

1. 获取到图片的 base64 格式；
2. 图片加载完成后，把图片转化为 canvas；
3. 使用 canvas 的 toDataURL 按自己的需要进行压缩；
4. 把 dataURL 转化成 blob 对象；
5. 把 blob 对象转化成 formData 对象，最后按照 ajax 接口调用方式提交；

## CSS

### `HSLA` 颜色

- `H`：Hue(色调)。0(或 360)表示红色，120 表示绿色，240 表示蓝色，也可取其他数值来指定颜色。取值为：0 - 360
- `S`：Saturation(饱和度)。取值为：0.0% - 100.0%
- `L`：Lightness(亮度)。取值为：0.0% - 100.0%
- `A`：Alpha 透明度。取值 0~1 之间。

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
