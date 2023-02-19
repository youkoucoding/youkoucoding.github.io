---
title: 'Curl 用法简介'
date: 2023-02-19T13:57:01+09:00
category:
  - devops
  - terminal tools
---


# curl 用法简介

> [Curl Cookbook](https://catonmat.net/cookbooks/curl)
> [curl 的用法指南](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html)

1. 不带任何参数，即 GET 请求

`curl https://www.google.com`


## 参数

### `-A`

`-A` 参数指定客户端的用户代理标头，即User-Agent。curl 的默认用户代理字符串是`curl/[version]`。

1. 将User-Agent修改为Chrome浏览器
   - `curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https://google.com`

2. 移除User-Agent
   - `curl -A "" https://www.google.com`

3. 通过`-H`参数直接指定标头，更改`User-Agent`
    - `curl -H "User-Agent:xxx" https://www.google.com`

### `-b`

- `-b`参数用来向服务器发送 `Cookie`
   - `curl -b 'foo=bar;foo2=bar2' https://google.com`

### `-c`

- `-c`参数将服务器设置的 Cookie 写入一个文件
   - `curl -c cookies.txt https://www.google.com`

### `-d`

- `-d`参数用于发送 `POST` 请求的数据体
   - `curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login`
   - 使用-d参数以后，HTTP 请求会自动加上标头`Content-Type : application/x-www-form-urlencoded`, 并且会自动将请求转为 POST 方法，因此可以省略`-X POST`
   - `-d`参数可以读取本地文本文件的数据，向服务器发送

### `--date-urlencode`

- `--data-urlencode`参数等同于`-d`，发送 POST 请求的数据体，区别在于会自动将发送的数据进行 URL 编码

### `-e`

- `-e`参数用来设置 HTTP 的标头`Referer`，表示请求的来源。
    - `curl -e 'https://www.google.com?q=example' https://www.google.com`
    - 上面命令将Referer标头设为https://google.com?q=example

- `-H`参数可以通过直接添加标头`Referer`，达到同样效果
    - `curl -H 'Referer: https://google.com?q=example' https://www.example.com`


### `-F`

- `-F`参数用来向服务器上传二进制文件
    - `curl -F 'file=@photo.png' https://google.com/profile`
    - 上面命令会给 HTTP 请求加上标头`Content-Type: multipart/form-data`，然后将文件`photo.png`作为file字段上传
    - `-F`参数可以指定 `MIME` 类型

### `-G`

- `-G`参数用来构造 URL 的查询字符串
    - `curl -G -d 'q=kitties' -d 'count=20' https://google.com/search`
    - 上面命令会发出一个 GET 请求，实际请求的 URL 为`https://google.com/search?q=kitties&count=20`。如果省略--G，会发出一个 POST 请求。
    - 如果数据需要 URL 编码，可以结合`--data--urlencode`参数。

### `-H`

- `-H`参数添加 HTTP 请求的标头。
    - `curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com`

### `-i`

- `-i`参数打印出服务器回应的 HTTP 标头。
    - 空一行，再输出网页的源码。

### `-I`

- `-I`参数向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来。
    - `curl -I https://www.example.com`
    - 上面命令输出服务器对 HEAD 请求的回应
    - `--head`参数等同于`-I`

### `-k`

- `-k`参数指定跳过 SSL 检测。
    - `curl -k https://www.example.com`
    - 上面命令不会检查服务器的 SSL 证书是否正确。

### `--limit-rate`

- `--limit-rate`用来限制 HTTP 请求和回应的带宽，模拟慢网速的环境
    - `curl --limit-rate 200k https://google.com`

### `-o`

- `-o`参数将服务器的回应保存成文件，等同于wget命令
    - `curl -o example.html https://www.example.com`
    - 上面命令将www.example.com保存成example.html

### `-O`

- `-O`参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名。
    - `curl -O https://www.example.com/foo/bar.html`
    - 上面命令将服务器回应保存成文件，文件名为`bar.html`

### `-s`

- `-s`参数将不输出错误和进度信息
    - `curl -s https://www.example.com`
    - 上面命令一旦发生错误，不会显示错误信息。不发生错误的话，会正常显示运行结果。
    - 如果想让 curl 不产生任何输出，可以使用下面的命令。
    - `curl -s -o /dev/null https://google.com`

### `-S`

- `-S`参数指定只输出错误信息，通常与-s一起使用。
    - `curl -s -o /dev/null https://google.com`
    - 上面命令没有任何输出，除非发生错误。
    - When used with -s it makes curl show an error message if it fails.
    
### `-u`

- `-u`参数用来设置服务器认证的用户名和密码。
    - `curl -u 'bob:12345' https://google.com/login`
    - 上面命令设置用户名为bob，密码为12345，然后将其转为 HTTP 标头Authorization: Basic Ym9iOjEyMzQ1
