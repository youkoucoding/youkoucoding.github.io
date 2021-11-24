---
title: '毎日のフロントエンド　 69'
date: 2021-11-24T17:19:04+09:00
description: frontend 每日一练
image: frontend-69-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十九日

## HTML

### **Question:** 怎样禁止表单记住密码自动填充

`autocomplete="off"` or `autocomplete="new-password"`

```html
<form method="post" action="/form">
  […]
  <div>
    <label for="cc">信用卡：</label>
    <input type="text" id="cc" name="cc" autocomplete="off" />
  </div>
</form>
```

## CSS

### **Question:** `*{box-sizing: border-box;}` 作用及好处有哪些

```css
{box-sizing: content-box（初期値）box-sizing: border-box}
```

盒子的尺寸计算有两种，一种是`content-box`，还有一种是`border-box`，两者的不同是计算最大尺寸时**是否包含框和内边距**

- `content-box` 的 `width` 不包括 `padding` 和 `border`
- `border-box` 的 `width` 包括 `padding` 和 `border` (更符合我们通常对一个「盒子」尺寸的认知,省掉一些计算)

## JavaScript

### **Question:** 对`base64`的理解，它的使用场景有哪些

Base64 是一种二进制到文本的编码方式： 一种将 byte 数组编码为字符串的方法，而且编码出的字符串只包含 ASCII 基础字符。

Base64 不是加密算法，其仅仅是一种编码方式，算法也是公开的，所以不能依赖它进行加密。

Base64 使用到的 64 个字符(加一个填充字符`=`)：

- `A-Z` 26 个
- `a-z` 26 个
- `0-9` 10 个
- `+` 1 个
- `/` 1 个

> Base64 就是为了解决各系统以及传输协议中二进制不兼容的问题而生的。

- Base-64 编码保证了二进制数据的安全

Base-64 编码可以将任意一组字节转换为较长的常见文本字符序列，从而可以合法地作为首部字段值。Base-64 编码将用户输入或二进制数据，打包成一种安全格式，将其作为 HTTP 首部字段的值发送出去，而无须担心其中包含会破坏 HTTP 分析程序的冒号、换行符或二进制值。

Base-64 编码是作为 MIME 多媒体电子邮件标准的一部分开发的，这样 MIME 就可以在不同的合法电子邮件网关之间传输富文本和任意的二进制数据里。Base-64 编码与将二进制数据文本化表示的 `uuencode`[^1] 和 `BinHex`[^2] 标准在本质上类似，但空间效率更高。

`Data URL` 用 `base64` 处理图片可以减少`http`请求

[^1]: [Uuencode - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-cn/Uuencode)
[^2]: [BinHex - Wikipedia](https://en.wikipedia.org/wiki/BinHex)

- `atob()`[^3]
- `btoa()`[^4]

[^3]: [WindowOrWorkerGlobalScope.atob() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/atob)
[^4]: [WindowOrWorkerGlobalScope.btoa() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/btoa)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[如何关闭表单自动填充 - Web 安全 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Security/Securing_your_site/Turning_off_form_autocompletion)

[Base64 - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
