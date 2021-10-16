---
title: '毎日のフロントエンド　30'
date: 2021-10-16T10:26:51+09:00
description: frontend 每日一练
image: frontend-30-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十日

## HTML

### **Question:** 网页上的验证码是为了解决什么问题？说说了解的验证码种类有哪些

1. 图形验证码
2. 字符验证码 文字+混淆 如早期的 7456 这种结果的验证码
3. 复杂字符验证码 复杂文字+混淆 如加入中文等本土化的增加识别难度
4. 计算验证码 数字+运算符+混淆 如 1+2=? 需要识别表达式增加识别难度
5. 精确识别 文字+混淆文字 如选出 优贝在线 中的 贝字，或者选出所有的筷子，所有的红绿灯（12306）
6. 滑动拼图验证 图像+滑块+图像凹槽 如常见的滑动拼图，提供商有易盾之类的
7. 拼图验证 图像+打乱 需要用户去拼合完成。teamviewer 和 google
8. 物理验证
9. 手机短信验证码
10. 手机语音验证码

## CSS

### **Question:** 描述下了解的图片格式及使用场景

| 格式 | 优点                                       | 缺点                               | 适用场景                        |
| ---- | ------------------------------------------ | ---------------------------------- | ------------------------------- |
| gif  | 文件小，支持动画、透明，无兼容性问题       | 只支持 256 种颜色                  | 色彩简单的 logo、icon、动图     |
| jpg  | 色彩丰富，文件小                           | 有损压缩，反复保存图片质量下降明显 | 色彩丰富的图片/渐变图像         |
| png  | 无损压缩，支持透明，简单图片尺寸小         | 不支持动画，色彩丰富的图片尺寸大   | logo/icon/透明图                |
| webp | 文件小，支持有损和无损压缩，支持动画、透明 | 浏览器兼容性不好                   | 支持 webp 格式的 app 和 webview |

▍PNG

- 优点：PNG 格式图片是无损压缩的图片，能在保证最不失真的情况下尽可能压缩图像文件的大小；图片质量高；色彩表现好；支持透明效果；提供锋利的线条和边缘，所以做出的 logo 等小图标效果会更好；更好地展示文字、颜色相近的图片。

- 缺点：占内存大,会导致网页加载速度慢；对于需要高保真的较复杂的图像，PNG 虽然能无损压缩，但图片文件较大，不适合应用在 Web 页面上。

- 适用场景：主要用于小图标或颜色简单对比强烈的小的背景图。

▍JPG

- 优点：占用内存小，网页加载速度快。

- 缺点：JPG 格式图片是有损压缩的图片，有损压缩会使原始图片数据质量下降，即 JPG 会在压缩图片时降低品质。

- 适用场景：由于这种格式图片对色彩表现比较好，所以适用于色彩丰富的图片。主要用于摄影作品或者大的背景图等。不合适文字比较多的图片。

▍SVG

- 优点：SVG 是矢量图形，不受像素影响，在不同平台上都表现良好；可以通过 JS 控制实现动画效果。

- 缺点：DOM 比正常的图形慢，而且如果其结点多而杂，就更慢；不能与 HTML 内容集成。

- 适用场景：主要用于设计模型的展示等。

▍WebP

- 优点：WebP 格式，谷歌（google）开发的一种旨在加快图片加载速度的图片格式。图片压缩体积大约只有 JPEG 的 2/3，并能节省大量的服务器宽带资源和数据空间。

- 缺点：相较编码 JPEG 文件，编码同样质量的 WebP 文件需要占用更多的计算资源。

- 适用场景：WebP 既支持有损压缩也支持无损压缩。将来可能是 JPEG 的代替品。

## JavaScript

### **Question:** 写一个方法判断字符串是否为回文字符串

> [Loading Question... - 力扣（LeetCode）](https://leetcode-cn.com/problems/valid-palindrome/)

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

输入: "A man, a plan, a canal: Panama"
输出: true
示例 2:

输入: "race a car"
输出: false

#### Solution

- 获取有效的字符串，我们利用正则去匹配字母和数字，因为忽略大小写，所以我们转成小写
- 然后利用 `split('')` 把字符串分割成数组，再用数组的 `reverse()` 去反转，再用 `join(‘’)` 去拼接
- 最后进行比较

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function (s) {
  if (s.length === 1) return true;
  const str = s.replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
  const strReverse = str.split('').reverse().join('');
  return str === strReverse;
};
```

## Reference

[[css] 描述下你所了解的图片格式及使用场景 · haizlin/fe-interview](https://github.com/haizlin/fe-interview/issues/107)