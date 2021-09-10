---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（三）
date: 2021-09-10T18:02:49+09:00
description: 知识点： RESTful API Design
image: restful-design.jpg
categories:
  - React
  - TypeScript
  - RESTful API
---

# 知识点五： Restful API 设计

## 1. Endpoint

表示 `API` 的具体地址

在 RESTful 架构中，每个网址代表一种资源（resource），所以网址中 **不能有动词，只能有名词**，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以 API 中的名词也应该使用复数。

```
# Example 应使用 HTTPs 协议

https://api.example.com/v1/books

https://api.example.com/v1/movies
```

## 2. HTTP METHOD

对于资源的具体操作，应由 HTTP 动词表示。

```
# 常用方法, (对应 SQL 语句)

GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
```

```
# 具体的使用实例
GET /zoos：列出所有动物园
POST /zoos：新建一个动物园
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
```

## 3. Filtering

如果记录数量很大，服务器不可能将全部数据返回，此时，`API` 应提供参数，用于过滤返回结果。

```
# 以下是一些常用参数

?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件
```

> 参数的设计允许存在冗余，例如：`GET /zoo/ID/animals` 等价于 `GET /animals?zoo_id=ID`

## 4. Status Code

```
# 服务器向用户返回的状态码和提示信息,常用状态码如下：
200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
```

> [幂等 - 术语表 | MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Idempotent)

> [HTTP response status codes - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

## 5. Error Handling

```
#  如果状态码是4xx，就应该向用户返回出错信息。
{
    error: "Invalid API key"
}
```

## 6. 返回结果的规范

```
GET /collection：返回资源对象的列表（数组）
GET /collection/resource：返回单个资源对象
POST /collection：返回新生成的资源对象
PUT /collection/resource：返回完整的资源对象
PATCH /collection/resource：返回完整的资源对象
DELETE /collection/resource：返回一个空文档
```

## 7. Hypermedia API (HATEOAS)

RESTful API 最好做到 Hypermedia，即返回结果中提供链接，连向其他 API 方法，使得用户不查文档，也知道下一步应该做什么。

```
# 比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。
{"link": {
  "rel":   "collection https://www.example.com/zoos",
  "href":  "https://api.example.com/zoos",
  "title": "List of zoos",
  "type":  "application/vnd.yourformat+json"
}}
```

Hypermedia API 的设计被称为 HATEOAS。Github 的 API 就是这种设计，访问 [https://api.github.com](https://api.github.com/) 会得到一个所有可用 API 的网址列表。

## Reference

[RESTful API 设计指南](https://www.ruanyifeng.com/blog/2014/05/restful_api.html)
