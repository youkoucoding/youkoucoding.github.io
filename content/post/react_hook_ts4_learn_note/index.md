---
title: React17 + ReactHook + TS4 仿Jira项目---学习笔记（一）
date: 2021-09-08T23:50:49+09:00
description: 这是一个知识点集合型的学习笔记
image: react-jira-cover.jpg
categories:
  - React
  - TypeScript
---

# 第一章

### 知识点一： 用 `create-react-app` 初始化项目：

#### 1. Quick Start

```shell
npx create-react-app react-jira --template typescript
# or
npm init react-app react-jira
# or
yarn create react-app react-jira

npx create-react-app my-app --use-npm # if you prefer to use npm
```

#### 2. Folder Structure

- For faster rebuilds, only files inside src are processed by webpack. You need to put any JS and CSS files inside src, otherwise webpack won’t see them.

  - 字体，图片等也同样应置于 scr 目录中
  - Only files inside public can be used from public/index.html.

  - ```js
    import React, { Component } from 'react';
    import './Button.css'; // Tell webpack that Button.js uses these styles

    class Button extends Component {
      render() {
        // You can use them as regular CSS styles
        return <div className='Button' />;
      }
    }
    ```

#### 3. Analizing the Bundle Size

```fish
npm install --save source-map-explorer
or
yarn add source-map-explorer
```

```json
// package.json

"scripts": {
+    "analyze": "source-map-explorer 'build/static/js/*.js'",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
}
```

### 知识点二： 用 Prettier 统一代码格式化风格

#### 1. Quick Start

```fish
yarn add --dev --exact prettier

echo {}> .prettierrc.json

# then create a .prettierignore file to let the Prettier CLI and editors know which files to not format.

# Ignore artifacts:
build
coverage

```

#### 2. Usage

##### In Command line

```fish
npx prettier --write .
# or
yarn prettier --write .
```

##### Setup In Editor

[Prettier set up in editor](https://prettier.io/docs/en/install.html#set-up-your-editor)

##### Git Hook (Pre-commit Hook)

[Pre-commit Hook · Prettier](https://prettier.io/docs/en/precommit.html)

##### Git Commitlint

[commitlint - Lint commit messages](https://commitlint.js.org/#/?id=getting-started)

commitlint checks if your commit messages meet the conventional commit format.

[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
[约定式提交](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

```js
// 常用类型 type
{
  "build": "Changes that affect the build system or external dependencies"
  "ci": "Changes to our CI configuration files and scripts"
  // "chore": "updating grunt tasks etc; no production code change",
  "docs": "Documentation only changes",
  "feat": "new feature for the user, not a new feature for build script",
  "fix": "A bug fix for user, not a fix to a build script",
  "perf": "A code change that impoves performance",
  "refactor": "A code change that neither fixes a bug nor adds a feature",
  "revert": "If the commit reverts a previous commit, it should begin with revert",
  "style": "Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons,etc)",
  "test": "Adding missing tests or correcting existing tests",
  "BREAKING CHANGE:"
};
```

`type(scope?): subject #scope is optional; multiple scopes are supported (current delimiter options: "/", "\" and ",") `

> scope

Example <scope> values: #

- init
- runner
- watcher
- config
- web-server
- proxy
- etc

> why use conventional commits

- Automatically generating CHANGELOGS
- Automatically determining a semantic version bump(base on the types of commits landed)
- Communicating the nature of changes to teammates, the public, and other stakeholders
- Triggering build and publish processes
- Making it easier for people to contribute to you projects, by allowing them to explore a more structured commit history

以上： 一个项目开始阶段的规范化配置。重点： Conventional Commits

### 知识点三： Json-server

[typicode/json-server: Get a full fake REST API with zero coding](https://github.com/typicode/json-server#table-of-contents)
