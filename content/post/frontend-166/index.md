---
title: '毎日のフロントエンド  166'
date: 2022-03-01T11:49:12+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `meta`的属性有哪些组成

```html
<!DOCTYPE html>
<!-- 使用 HTML5 doctype，不区分大小写 -->
<html lang="zh-cmn-Hans">
  <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->
  <head>
    <!-- 声明文档使用的字符编码 -->
    <meta charset="utf-8" />
    <!-- 优先使用 IE 最新版本和 Chrome -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <!-- 页面描述 -->
    <meta name="description" content="不超过150个字符" />
    <!-- 页面关键词 -->
    <meta name="keywords" content="" />
    <!-- 网页作者 -->
    <meta name="author" content="name, email@gmail.com" />
    <!-- 搜索引擎抓取 -->
    <meta name="robots" content="index,follow" />
    <!-- 为移动设备添加 viewport -->
    <meta
      name="viewport"
      content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no"
    />
    <!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->

    <!-- iOS 设备 begin -->
    <meta name="apple-mobile-web-app-title" content="标题" />
    <!-- 添加到主屏后的标题（iOS 6 新增） -->
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <!-- 是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏 -->

    <meta
      name="apple-itunes-app"
      content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"
    />
    <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->
    <meta name="apple-mobile-web-app-status-bar-style" content="black" />
    <!-- 设置苹果工具栏颜色 -->
    <meta name="format-detection" content="telphone=no, email=no" />
    <!-- 忽略页面中的数字识别为电话，忽略email识别 -->
    <!-- 启用360浏览器的极速模式(webkit) -->
    <meta name="renderer" content="webkit" />
    <!-- 避免IE使用兼容模式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <!-- 不让百度转码 -->
    <meta http-equiv="Cache-Control" content="no-siteapp" />
    <!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
    <meta name="HandheldFriendly" content="true" />
    <!-- 微软的老式浏览器 -->
    <meta name="MobileOptimized" content="320" />
    <!-- uc强制竖屏 -->
    <meta name="screen-orientation" content="portrait" />
    <!-- QQ强制竖屏 -->
    <meta name="x5-orientation" content="portrait" />
    <!-- UC强制全屏 -->
    <meta name="full-screen" content="yes" />
    <!-- QQ强制全屏 -->
    <meta name="x5-fullscreen" content="true" />
    <!-- UC应用模式 -->
    <meta name="browsermode" content="application" />
    <!-- QQ应用模式 -->
    <meta name="x5-page-mode" content="app" />
    <!-- windows phone 点击无高光 -->
    <meta name="msapplication-tap-highlight" content="no" />
    <!-- iOS 图标 begin -->
    <link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" />
    <!-- iPhone 和 iTouch，默认 57x57 像素，必须有 -->
    <link
      rel="apple-touch-icon-precomposed"
      sizes="114x114"
      href="/apple-touch-icon-114x114-precomposed.png"
    />
    <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->
    <link
      rel="apple-touch-icon-precomposed"
      sizes="144x144"
      href="/apple-touch-icon-144x144-precomposed.png"
    />
    <!-- Retina iPad，144x144 像素，可以没有，但推荐有 -->
    <!-- iOS 图标 end -->

    <!-- iOS 启动画面 begin -->
    <link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png" />
    <!-- iPad 竖屏 768 x 1004（标准分辨率） -->
    <link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png" />
    <!-- iPad 竖屏 1536x2008（Retina） -->
    <link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png" />
    <!-- iPad 横屏 1024x748（标准分辨率） -->
    <link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png" />
    <!-- iPad 横屏 2048x1496（Retina） -->

    <link rel="apple-touch-startup-image" href="/splash-screen-320x480.png" />
    <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->
    <link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png" />
    <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->
    <link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png" />
    <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->
    <!-- iOS 启动画面 end -->

    <!-- iOS 设备 end -->
    <meta name="msapplication-TileColor" content="#000" />
    <!-- Windows 8 磁贴颜色 -->
    <meta name="msapplication-TileImage" content="icon.png" />
    <!-- Windows 8 磁贴图标 -->

    <link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml" />
    <!-- 添加 RSS 订阅 -->
    <link rel="shortcut icon" type="image/ico" href="/favicon.ico" />
    <!-- 添加 favicon icon -->

    <!-- sns 社交标签 begin -->
    <!-- 参考微博API -->
    <meta property="og:type" content="类型" />
    <meta property="og:url" content="URL地址" />
    <meta property="og:title" content="标题" />
    <meta property="og:image" content="图片" />
    <meta property="og:description" content="描述" />
    <!-- sns 社交标签 end -->

    <title>标题</title>
  </head>
</html>
```

> meta 是 html 语言 head 区的一个辅助性标签。meta 标签的作用有：搜索引擎优化（seo），定义页面使用语言，自动刷新并指向新的页面，实现网页转换时的动态效果，控制页面缓冲，网页定级评价，控制网页显示的窗口等

- meta 标签共有两个属性，分别是`http-equiv`属性和`name`属性，不同的属性又有不同的参数值，这些不同的参数值就实现了不同的网页功能

  1. name 属性

     - `name` 属性主要用于描述网页，与之对应的属性值为 `content`，`content` 中的内容主要是便于搜索引擎机器人查找信息和分类信息用的 `<meta name="参数" content="具体的参数值">`
       - `keywords`用来告诉搜索引擎网页的关键字
         - `<meta name="keywords" content="meta总结,html meta,meta属性"> `
       - `description`用来告诉搜索引擎网站主要内容
         - `<meta name="description"content="haorooms博客,html的meta总结，meta是html语言head区的一个辅助性标签。">`
       - `robots`用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引`<meta name="robots"content="none">`
         - `content的`参数有`all`,`none`,`index`,`noindex`,`follow`,`nofollow`。默认是`all`
           - all：文件将被检索，且页面上的链接可以被查询；
           - none：文件将不被检索，且页面上的链接不可以被查询；
           - index：文件将被检索；
           - follow：页面上的链接可以被查询；
           - noindex：文件将不被检索，但页面上的链接可以被查询；
           - nofollow：文件将被检索，但页面上的链接不可以被查询；
       - `author`
         - `<meta name="author"content="root,root@xxxx.com">`
       - `generator`的信息参数，网站的采用的什么软件制作
       - `COPYRIGHT`的信息参数，网站版权信息

  2. `http-equiv`属性
     - http 的文件头作用，可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为`content`，
       content 中的内容是各个参数的变量值。
       - `Expires`(期限)
         - 可以用于设定网页的到期时间。一旦网页过期，必须到服务器上重新传输(必须使用 GMT 的时间格式)
       - Pragma(cache 模式)
         - 禁止浏览器从本地计算机的缓存中访问页面内容
       - `Refresh`(刷新)
         - 自动刷新并指向新页面 `<meta http-equiv="Refresh"content="2;URL=http://www.helloworld.com">`
       - `Set-Cookie`(cookie 设定)
         - 如果网页过期，那么存盘的 cookie 将被删除`<meta http-equiv="Set-Cookie"content="cookie value=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">`
         - ...

## CSS

### 引起 Reflow 和 Repaint 的操作

- `Reflow` 当 Render Tree 中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档
  - 页面首次渲染
  - 浏览器窗口大小发生改变
  - 元素尺寸或位置发生改变
  - 元素内容变化（文字数量或图片大小等等）
  - 元素字体大小变化
  - 添加或者删除可见的 DOM 元素
  - 激活 CSS 伪类（例如：:hover）
  - 查询某些属性或调用某些方法
- `Repaint`
  - 当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility 等），浏览器会将新样式赋予给元素并重新绘制它

## JavaScript

### 下载一个 zip 文件

- `<a>`标签加`download`属性

  - download:指定下载文件的文件名
  - `<a href="http://somehost/somefile.zip" download="filename.zip">Download file</a>`

- 文件流的方式

```js
let a = document.createElement('a');
let url = window.URL.createObjectURL(blob);
let filename = 'what-you-want.txt';
a.href = url;
a.download = filename;
a.click();
window.URL.revokeObjectURL(url);
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[meta 属性整理](https://www.jianshu.com/p/04a4021b2a44)

[lang="zh" 还是 lang="zh-cn"？](https://www.zhihu.com/question/20797118?utm_source=weibo&utm_medium=weibo_share&utm_content=share_question&utm_campaign=share_sidebar)

[Metatags SEO & Inbound Marketing | How to rank with the search engines](https://www.metatags.org/)

[URL.revokeObjectURL() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/revokeObjectURL)
