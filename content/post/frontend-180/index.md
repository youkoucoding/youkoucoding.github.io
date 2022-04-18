---
title: '毎日のフロントエンド  180'
date: 2022-03-18T17:03:46+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `track`标签

- `track`元素可以为使用`video`元素播放的视频或使用`audio`元素播放的音频添加字幕。写在`video`或`audio`元素内部。

- 如果使用标记描述媒体文件，则标记必须被书写在标记之后。track 元素是一个空元素，其开始标记与结束标记之间不包含任何内容。

- 属性
  - `default`：默认轨道。（值：default）。default 属性用于通知浏览器在用户没有选择使用其他字幕文件的时候可以使用当前 track 文件
  - `kind`：文本轨道的文本类型（值：captions、chapters、descriptions、metadata、subtitles）。kind 属性用于指定字幕文件(即用于存放字幕、章节标题、说明文字或元数据的文件) 的种类。可以对 kind 属性指定的属性值为 subtitles、captions、descriptions、chapters 与 metadata
  - `label`：文本轨道的标签和标题（值：text）。
  - `src`：轨道文件的 URL，必选属性（值：URL）。src 属性用于指定字幕文件的存放路径，该属性是一个必须使用的属性。src 属性的属性值可以是一个绝对 URL 路径，也可以是一个相对 URL 路径。
  - `srclang`：轨道文本数据的语言。（值：language_code ）。srclang 属性用于指定字幕文件的语言。例如，srclang="en" 和 srclang="zh-cn"分别表示字幕文件为英语和汉语。

## CSS

### 用 css 画一个圆

`<div class="circle"></div>`

1. `border-radius`

```css
.cirlce {
  width: 10vw;
  height: 10vw;
  background: gray;
  border-radius: 50%;
}
```

2. `clip-path`

```css
.circle {
  width: 10vw;
  height: 10vw;
  background: gray;
  clip-path: circle();
}
```

3. `svg background`

```css
.circle {
  width: 10vw;
  height: 10vw;
  background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Ccircle cx='50%25' cy='50%25' r='50%25' fill='gray'/%3E%3C/svg%3E");
}
```

4. `radial-gradient`

```css
.circle {
  width: 10vw;
  height: 10vw;
  background: radial-gradient(gray 70%, transparent 70%);
}
```

5. `font`

```css
.circle::after {
  content: '●';
  font-size: 10vw; //字体实际大小
  line-height: 1;
}
```

6. `mix-blend-mode`

```css
.circle {
  width: 10vw;
  height: 10vw;
  background: gray;
}
.circle::after {
  content: '';
  display: block;
  width: 10vw;
  height: 10vw;
  mix-blend-mode: lighten;
  background: radial-gradient(#000 70%, #fff 70%);
}
```

## Tips

### HTTP 协议概览

#### `HTTP/0.9`

- 协议定义了客户端发起请求、服务端响应请求的通信模式。请求报文内容只有 1 行，为 GET 加上请求的文件路径。服务端收到请求后返回一个以 ASCII 字符流编码的 HTML 文档。

#### `HTTP/1.0`

- 最核心的改变是增加了头部设定，头部内容以键值对的形式设置。
- 请求头部通过 `Accept` 字段来告诉服务端可以接收的文件类型
- 响应头部再通过 `Content-Type` 字段来告诉浏览器返回文件的类型。
- 其他很多功能也可以依靠头部字段实现，比如缓存、认证信息。

---

- HTTP/1.0 每进行一次通信，都需要经历建立连接、传输数据和断开连接三个阶段。当一个页面引用了较多的外部文件时，这个建立连接和断开连接的过程就会增加大量网络开销。

#### `HTTP/1.1`

- HTTP/1.1 版本增加了一个创建持久连接的方法。主要实现是当一个连接传输完成时，并不是马上进行关闭，而是继续复用它传输其他请求的数据，这个连接保持到浏览器或者服务器要求断开连接为止。

##### TCP 是怎样建立/断开连接的

###### 三次握手

- **第一次握手**：刚开始客户端处于 CLOSED 的状态，服务端处于 LISTEN 状态。客户端给服务端发送一个 SYN 报文，并指明客户端的初始化序列号 ISN，此时客户端处于 SYN_SEND 状态。

- **第二次握手**：当服务器收到客户端的 SYN 报文之后，会以自己的 SYN 报文作为应答，并且也指定了自己的初始化序列号 ISN。同时会把客户端的 ISN + 1 作为 ACK 的值，表示自己已经收到了客户端的 SYN，此时服务器处于 SYN_REVD 的状态。

- **第三次握手**：当客户端收到 SYN 报文之后，会发送一个 ACK 报文，当然，也同样把服务器的 ISN + 1 作为 ACK 的值，表示已经收到了服务端的 SYN 报文，此时客户端处于 ESTABLISHED 状态。服务器收到 ACK 报文之后，也处于 ESTABLISHED 状态，此时，双方成功建立起了连接。

> 第一次握手成功让服务端知道了客户端具有发送能力，第二次握手成功让客户端知道了服务端具有接收和发送能力，但此时服务端并不知道客户端是否接收到了自己发送的消息，所以第三次握手就起到了这个作用。经过三次通信后，服务端和客户端都确认了双方的接收和发送能力。

###### 四次挥手

> 当客户端和服务端断开连接时要发送四次数据，这个过程称之为四次挥手

- **第一次挥手**：在挥手之前服务端与客户端都处于 ESTABLISHED 状态。客户端发送一个 FIN 报文，用来关闭客户端到服务器的数据传输，此时客户端处于 FIN_WAIT_1 状态。

- **第二次挥手**：当服务端收到 FIN 之后，会发送 ACK 报文，并且把客户端的序列号值加 1 作为 ACK 报文的序列号值，表明已经收到客户端的报文了，此时服务端处于 CLOSE_WAIT 状态。

- **第三次挥手**：如果服务端同意关闭连接，则会向客户端发送一个 FIN 报文，并且指定一个序列号，此时服务端处于 LAST_ACK 的状态。

- **第四次挥手**：当客户端收到 ACK 之后，处于 FIN_WAIT_2 状态。待收到 FIN 报文时发送一个 ACK 报文作为应答，并且把服务端的序列号值 +1 作为自己 ACK 报文的序列号值，此时客户端处于 TIME_WAIT 状态。等待一段时间后会进入 CLOSED 状态，当服务端收到 ACK 报文之后，也会变为 CLOSED 状态，此时连接正式关闭。

---

- 为什么建立连接只通信了三次，而断开连接却用了四次？
  - 因为当服务端收到客户端的 FIN 报文后，发送的 ACK 报文只是用来应答的，并不表示服务端也希望立即关闭连接。
  - 当只有服务端把所有的报文都发送完了，才会发送 FIN 报文，告诉客户端可以断开连接了，因此在断开连接时需要四次挥手。

#### `HTTP/2`

> HTTP/1.1 虽然通过长连接减少了大量创建/断开连接造成的性能消耗，但由于它的并发能力受到限制，所以传输性能还有很大提升空间。

- 为什么说 HTTP/1.1 的并发能力受限？
  - 浏览器为了减轻服务器的压力，限制了同一个域下的 HTTP 连接数，即 6 ~ 8 个，所以在 HTTP/1.1 下很容易看到资源文件等待加载的情况，**对应优化的方式就是使用多个域名来加载图片资源；**
  - HTTP/1.1 本身的问题，虽然 HTTP/1.1 中使用持久连接时，多个请求能共用一个 TCP 连接，但在一个连接中同一时刻只能处理一个请求，在当前的请求没有结束之前，其他的请求只能处于阻塞状态，这种情况被称为 “队头阻塞” 。
  - _2015 年正式发布的 HTTP/2 中新增了一个二进制分帧的机制来提升传输效率_

---

- HTTP/2 将默认不再使用 ASCII 编码传输，而是改为二进制数据。
- 客户端在发送请求时会将每个请求的内容封装成不同的带有编号的二进制帧，然后将这些帧同时发送给服务端。
- 服务端接收到数据之后，会将相同编号的帧合并为完整的请求信息。
- 服务端返回结果、客户端接收结果也遵循这个帧的拆分与组合的过程。

- 受益于二进制分帧，对于同一个域，客户端只需要与服务端建立一个连接即可完成通信需求，_也不再受限于浏览器的连接数限制了_，这种利用一个连接来发送多个请求的方式称为“多路复用”。
- HTTP/2 也增加了一些其他的功能，比如通过压缩头部信息来减少传输体积，以及通过服务推送来减少客户端请求。

#### `HTTP/3`

> HTTP/2 也并非完美，考虑一种情况，如果客户端或服务端在通信时出现数据包丢失，或者任何一方的网络出现中断，那么整个 TCP 连接就会暂停。

- HTTP/2 由于采用二进制分帧进行多路复用，通常只使用一个 TCP 连接进行传输，在丢包或网络中断的情况下后面的所有数据都被阻塞。但对于 HTTP/1.1 来说，可以开启多个 TCP 连接，任何一个 TCP 出现问题都不会影响其他 TCP 连接，剩余的 TCP 连接还可以正常传输数据。这种情况下 HTTP/2 的表现就不如 HTTP/1 了

- 2018 年 HTTP/3 将底层依赖的 TCP 改成 UDP，从而彻底解决了这个问题。UDP 相对于 TCP 而言最大的特点是传输数据时不需要建立连接，可以同时发送多个数据包，所以传输效率很高，缺点就是没有确认机制来保证对方一定能收到数据。

---

| 协议版本 | 解决的核心问题           | 解决方式                               |
| -------- | ------------------------ | -------------------------------------- |
| 0.9      | HTML 文件传输            | 确立了客户端请求、服务端响应的通信流程 |
| 1.0      | 不同类型文件传输         | 设立头部字段                           |
| 1.1      | 创建/断开 TCP 连接开销大 | 建立长连接进行复用                     |
| 2        | 并发数有限               | 二进制分帧                             |
| 3        | TCP 丢包阻塞             | 采用 UDP 协议                          |

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<track>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/track)