---
title: '毎日のフロントエンド  130'
date: 2022-01-24T14:10:51+09:00
description: frontend 每日一练
image: frontend-130-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十日

## HTML

### **Question:** 锚点的作用是什么？怎么创建一个锚点

- 基于 window 对象，查找 dom 对应`id`的元素就是锚点。操作该元素组成的 dom，用 js 改变视图层
- 在 HTML 中定义`id`，用`document.queryselector`找到该元素，既可以视作锚点

## JavaScript

### **Question:** 实现一个全屏的功能

`Element.requestFullscreen()`

调用此 API 并不能保证元素一定能够进入全屏模式。如果元素被允许进入全屏幕模式，返回的 Promise 会 resolve，并且该元素会收到一个 fullscreenchange (en-US)事件，通知它已经进入全屏模式。如果全屏请求被拒绝，返回的 promise 会变成 rejected 并且该元素会收到一个 fullscreenerror (en-US)事件。如果该元素已经从原来的文档中分离，那么该文档将会收到这些事件。

[sindresorhus/screenfull: Simple wrapper for cross-browser usage of the JavaScript Fullscreen API](https://github.com/sindresorhus/screenfull)

## Clean Code

### Writing readable Functions

#### Principle 1 - Small

The marjority of your functions should be less than than 15 lines long, and they should hardly ever be more than 20 lines long.

You should always be able to fit the entire function on your monitor. If you can't then you probably need to break the function up into separate functions.

It means that your function will not be large enough to hold several nested structures. You won't be able to fit a loop, and then a nested if/else statement inside that loop, etc.

Instead, you should make those nested structures separate function calls. This also adds documentary value since those functions will have nice descriptive names that explain what they do.(描述性函数名)

#### Principle 2 - Do One Thing

Another way of stating Principle 1 is that your function should only do one thing.

A good heuristic to use is to see if you can extract some code from your function and turn that code into a new function with a name that is not just a restatement of its implementation.

If you can do that, then that suggests that your function is doing multiple things and you should break in down further.

#### Principle 3 - One level of Abstraction Per Function

Your programs can frequently be divided into layers of abstraction, where each layer represents a different model of the same information and processes, but in a different manner.

An example is if you're developing a program to scrape a website, parse some data and then put that data in a database.

**One layer of abstraction** is the HTML string you get after sending an HTTP request to the website’s server.

You take that HTML string and send it to your second layer of abstraction, where an HTML parser converts that string into an object where the HTML page is represented as a nested data structure (read up on BeautifulSoup if you’d like a more concrete example). This is **the second layer** of abstraction.

**The third layer** of abstraction could be where you extract the specific data you need from the HTML page object and store that data in an array.

**The fourth layer** of abstraction would be where you write the data in that array to a database.

**The statements in a function should all be at the same level of abstraction.**

Clean Code suggests The Stepdown Rule, where your code can be read as a top-down narrative.

Every function should be followed by a function at the next layer of abstraction. This way, as you read the program, you’ll be descending down one layer of abstraction at a time as you go through the list of functions.

#### Principle 4 - Use Descriptive Names

**Don’t be afraid to make your names long.** A long, descriptive name is much better than a short, ambiguous name. Your editor’s autocomplete function should make it easy to write code with long function names.

Also, use a naming convention that allows multiple words to be easily read in function names. CamelCase is one example.

#### Principle 5 - Prefer Fewer Function Arguments

The ideal number of arguments for a function is zero. Having more than three arguments for a function should be avoided.

Having lots of function arguments makes it harder to read and understand the function since it forces you to know extra details (and those details are usually from a different layer of abstraction).

Additionally, having lots of function arguments makes it more difficult for you to write tests. You have to write all the test cases to ensure that all the various combinations of arguments work properly.

#### Principle 6 - Avoid Side Effects

A Side Effect is where your function modifies some state variable outside of its local environment, **in addition to returning a value (the intended effect).**

How Grab Processes Billions of Events in Real Time
Clean Code's recommendations on how to write readable functions. Plus, a free textbook on data mining, a collection of design patterns for building apps in the cloud and more.
Jan 21

Comment
Share
Hey Everyone,

Today we’ll be talking about

Clean Code’s recommendations on how to write readable functions.

layers of abstraction in your code

descriptive function names

number of arguments

avoiding side effects in your functions

How Grab Processes Billions of Events in Real Time

Trident is an IFTTT engine that ingests billions of events on the Grab platform and triggers predefined actions based off those events

Grab talks about their design goals with Trident and how they implemented it

They talk about how they scale the server level and the data storage level of Trident to cope with variable demand

Plus some awesome tech snippets on

A style guide for better Git commit messages

A programmer’s guide to data mining

A collection of design patterns for building reliable, scalable and secure applications in the cloud

How much it costs to run Lichess (the most popular free chess site on the internet)

If you find Quastor useful, you should check out Pointer.io!

It’s a reading club for software developers that sends out super high quality engineering-related content.

It’s read by CTOs, Engineering Managers and Senior Engineers so you should definitely sign up if you’re on that path (or if you want to go down that path in the future).

It’s completely free!

Check out Pointer.io!

sponsored (but I’m a big fan of Pointer)

Clean Code - writing readable Functions
Here’s a summary of Clean Code’s advice on writing readable functions.

This advice is geared towards functions written in an OOP language, although many of the concepts carry over to other programming paradigms.

Principle 1 - Small!
The majority of your functions should be less than 15 lines long, and they should hardly ever be more than 20 lines long.

You should always be able to fit the entire function on your monitor. If you can’t then you probably need to break the function up into separate functions.

If you’re following this principle, it means that your function will not be large enough to hold several nested structures. You won’t be able to fit a loop, and then a nested if/else statement inside that loop, etc.

Instead, you should make those nested structures separate function calls. This also adds documentary value since those functions will have nice descriptive names that explain what they do.

Principle 2 - Do One Thing
Another way of stating Principle 1 is that your function should only do one thing.

A good heuristic to use is to see if you can extract some code from your function and turn that code into a new function with a name that is not just a restatement of its implementation.

If you can do that, then that suggests that your function is doing multiple things and you should break it down further.

Principle 3 - One Level of Abstraction Per Function
Your programs can frequently be divided into layers of abstraction, where each layer represents a different model of the same information and processes, but in a different manner.

An example is if you’re developing a program to scrape a website, parse some data and then put that data in a database.

One layer of abstraction is the HTML string you get after sending an HTTP request to the website’s server.

You take that HTML string and send it to your second layer of abstraction, where an HTML parser converts that string into an object where the HTML page is represented as a nested data structure (read up on BeautifulSoup if you’d like a more concrete example). This is the second layer of abstraction.

The third layer of abstraction could be where you extract the specific data you need from the HTML page object and store that data in an array.

The fourth layer of abstraction would be where you write the data in that array to a database.

The statements in a function should all be at the same level of abstraction.

You should not be manipulating the HTML string object and writing to the database in the same function.

Instead, Clean Code suggests The Stepdown Rule, where your code can be read as a top-down narrative.

Every function should be followed by a function at the next layer of abstraction. This way, as you read the program, you’ll be descending down one layer of abstraction at a time as you go through the list of functions.

Principle 4 - Use Descriptive Names
If you’re following the first three principles, then coming up with a name shouldn’t be too difficult.

The smaller and more focused your functions are, the easier it is to come up with a name.

Don’t be afraid to make your names long. A long, descriptive name is much better than a short, ambiguous name. Your editor’s autocomplete function should make it easy to write code with long function names.

Also, use a naming convention that allows multiple words to be easily read in function names. CamelCase is one example.

Principle 5 - Prefer Fewer Function Arguments
The ideal number of arguments for a function is zero. Having more than three arguments for a function should be avoided.

Having lots of function arguments makes it harder to read and understand the function since it forces you to know extra details (and those details are usually from a different layer of abstraction).

Additionally, having lots of function arguments makes it more difficult for you to write tests. You have to write all the test cases to ensure that all the various combinations of arguments work properly!

Principle 6 - Avoid Side Effects
A Side Effect is where your function modifies some state variable outside of its local environment, in addition to returning a value (the intended effect).

An example might be a function that reads a certain value from a database and then returns that value.

If that same function also modifies state in some way (and that modification lasts after the function terminates), then that modification is a side effect.

Clean Code recommends you avoid side effects.

Your function should either change the state of an object(s), or it should return some information. **A single function should not do both!**

#### Principle 7 - Use Exceptions, not Error Codes

Your functions should rarely return error codes. Instead, if something goes wrong, you should throw an exception with a well-written error message.

One issue with error codes is that it leads to deeply nested structures. When you return an error code, the caller must deal with the error code immediately.

On the other hand, a caller can separate the exception handler logic from the other code (a try, catch statement with separate try logic and catch logic).

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Element.requestFullscreen() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/requestFullScreen)
