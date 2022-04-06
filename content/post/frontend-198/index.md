---
title: '毎日のフロントエンド 198'
date: 2022-04-05T22:53:47+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### CSS 实现段落首字母或首字放大效果

1. `::first-letter` 伪元素

```css
div::first-letter {
  font-size: 24px;
  font-weight: bold;
}
```

2. `text-transform`

```html
<style>
  div {
    text-transform: capitalize;
  }
</style>
<div>come on</div>
```

## Tips

### `npm ci`

#### 合理使用 npm ci 和 npm install

- npm ci 就是专门为 CI 环境准备的安装命令，相比 npm install 它的不同之处在于：
  - `npm ci` 要求项目中必须存在 package-lock.json 或 npm-shrinkwrap.json；
  - `npm ci` 完全根据 package-lock.json 安装依赖，这可以保证整个开发团队都使用版本完全一致的依赖；
  - `npm ci` 完全根据 package-lock.json 安装依赖，在安装过程中，它不需要计算求解依赖满足问题、构造依赖树，因此安装过程会更加迅速；
  - `npm ci` 在执行安装时，会先删除项目中现有的 node_modules，然后全新安装；
  - `npm ci` 只能一次安装整个项目所有依赖包，无法安装单个依赖包；
  - `如果 pac`kage-lock.json 和 package.json 冲突，那么 npm ci 会直接报错，并非更新 lockfiles；
  - `npm ci` 永远不会改变 package.json 和 package-lock.json。

> CI 环境使用 npm ci 代替 npm install，一般会获得更加稳定、一致和迅速的安装体验。

#### 使用 `package-lock.json` 优化依赖安装时间

- 对于应用项目，应上传 `package-lock.json` 到仓库中，以保证依赖安装的一致性
- 如果项目中使用了 package-lock.json 一般还可以显著*加速*依赖安装时间
- 因为 package-lock.json 中**已经缓存了每个包的具体版本和下载链接**，你不需要再去远程仓库进行查询，即可直接进入文件完整性校验环节，减少了大量网络请求

- **除了上面所述内容，CI 环境上，缓存 node_modules 文件也是企业级使用包管理工具常用的优化做法。**

> package-lock.json 文件的作用是锁定依赖安装结构，目的是保证在任意机器上执行 npm install 都会得到完全相同的 node_modules 安装结果

- 为什么单一的 package.json 不能确定唯一的依赖树：
  - 不同版本的 npm 的安装依赖策略和算法不同；
  - npm install 将根据 package.json 中的 semver-range version 更新依赖，某些依赖项自上次安装以来，可能已发布了新版本。

---

- 一个 package-lock.json 的 dependency 主要由以下部分构成：
  - Version：依赖包的版本号
  - Resolved：依赖包安装源（可简单理解为下载地址）
  - Integrity：表明包完整性的 Hash 值
  - Dev：表示该模块是否为顶级模块的开发依赖或者是一个的传递依赖关系
  - requires：依赖包所需要的所有依赖项，对应依赖包 package.json 里 dependencies 中的依赖项
  - dependencies：依赖包 node_modules 中依赖的包（特殊情况下才存在）（**只有子依赖的依赖和当前已安装在根目录的 node_modules 中的依赖冲突之后，才会有这个属性**）

---

- 如果开发一个应用，我建议把 package-lock.json 文件提交到代码版本仓库。这样可以保证项目组成员、运维部署成员或者 CI 系统，在执行 npm install 后，能得到完全一致的依赖安装内容。

- 如果目标是开发一个给外部使用的库，那就要谨慎考虑，因为库项目一般是被其他项目依赖的，在不使用 package-lock.json 的情况下，就可以复用主项目已经加载过的包，减少依赖重复和体积。

- 如果开发的库依赖了一个精确版本号的模块，那么提交 lockfiles 到仓库可能会造成同一个依赖不同版本都被下载的情况。如果作为库开发者，真的有使用某个特定版本依赖的需要，一个更好的方式是定义 peerDependencies。

> 一个推荐的做法是：把 package-lock.json 一起提交到代码库中，不需要 ignore。但是执行 npm publish 命令，发布一个库的时候，它应该被忽略而不是直接发布出去。

- 对于 lockfiles 处理的几个提示：

1. 早期 npm 锁定版本的方式是使用 npm-shrinkwrap.json，它与 package-lock.json 不同点在于：npm 包发布的时候默认将 npm-shrinkwrap.json 发布，因此类库或者组件需要慎重。

2. 使用 package-lock.json 是 npm v5.x 版本新增特性，而 npm v5.6 以上才逐步稳定，在 5.0 - 5.6 中间，对 package-lock.json 的处理逻辑进行过几次更新。

3. 在 npm v5.0.x 版本中，npm install 时都会根据 package-lock.json 文件下载，不管 package.json 内容究竟是什么。

4. npm v5.1.0 版本到 npm v5.4.2，npm install 会无视 package-lock.json 文件，会去下载最新的 npm 包并且更新 package-lock.json。

5. npm 5.4.2 版本后：
   - 如果项目中只有 package.json 文件，npm install 之后，会根据它生成一个 package-lock.json 文件；
   - 如果项目中存在 package.json 和 package-lock.json 文件，同时 package.json 的 semver-range 版本 和 package-lock.json 中版本兼容，即使此时有新的适用版本，npm install 还是会根据 package-lock.json 下载；
   - 如果项目中存在 package.json 和 package-lock.json 文件，同时 package.json 的 semver-range 版本和 package-lock.json 中版本不兼容，npm install 时 package-lock.json 将会更新到兼容 package.json 的版本；
   - 如果 package-lock.json 和 npm-shrinkwrap.json 同时存在于项目根目录，package-lock.json 将会被忽略。

---

- npm 设计了以下几种依赖类型声明：

  - `dependencies` 项目依赖
  - `devDependencies` 开发依赖
  - `peerDependencies` 同版本依赖
  - `bundledDependencies` 捆绑依赖
  - `optionalDependencies` 可选依赖

  - `dependencies` 表示项目依赖，这些依赖都会成为线上生产环境中的代码组成部分。当它关联的 npm 包被下载时，dependencies 下的模块也会作为依赖，一起被下载。
  - `devDependencies` 表示开发依赖，不会被自动下载，因为 devDependencies 一般只在开发阶段起作用或只是在开发环境中需要用到。比如 Webpack，预处理器 babel-loader、scss-loader，测试工具 E2E、Chai 等，这些都是辅助开发的工具包，无须在生产环境使用。
  - 并不是只有在 dependencies 中的模块才会被一起打包，而在 devDependencies 中的依赖一定不会被打包。实际上，依赖是否被打包，完全取决于项目里是否被引入了该模块。dependencies 和 devDependencies 在业务中更多的只是一个规范作用，我们自己的应用项目中，使用 npm install 命令安装依赖时，dependencies 和 devDependencies 内容都会被下载。
  - peerDependencies 表示同版本依赖，简单来说就是：如果安装 A，那么也安装 A 对应的依赖。

- peerDependencies 主要的使用场景。这类场景有以下特点：

  - 插件不能单独运行
  - 插件正确运行的前提是核心依赖库必须先下载安装
  - 不希望核心依赖库被重复下载
  - 插件 API 的设计必须要符合核心依赖库的插件编写规范
  - 在项目中，同一插件体系下，核心依赖库版本最好相同

- bundledDependencies 和 npm pack 打包命令有关

  - 在 bundledDependencies 中指定的依赖包，必须先在 dependencies 和 devDependencies 声明过，否则在 npm pack 阶段会进行报错。

- optionalDependencies 表示可选依赖，就是说即使对应依赖项安装失败了，也不会影响整个安装过程。一般很少使用到它，也不建议使用，因为大概率会增加项目的不确定性和复杂性。

---

#### npm 最佳实操建议

1. 优先使用 npm v5.4.2 以上的 npm 版本，以保证 npm 的最基本先进性和稳定性。

2. 项目的第一次搭建使用 npm install 安装依赖包，并提交 package.json、package-lock.json，而不提交 node_modules 目录。

3. 其他项目成员首次 checkout/clone 项目代码后，执行一次 npm install 安装依赖包。

4. 对于升级依赖包的需求：

   - 依靠 npm update 命令升级到新的小版本；
   - 依靠 npm install @ 升级大版本；
   - 也可以手动修改 package.json 中版本号，并执行 npm install 来升级版本；
   - 本地验证升级后新版本无问题，提交新的 package.json、package-lock.json 文件。

5. 对于降级依赖包的需求：执行 npm install @ 命令，验证没问题后，提交新的 package.json、package-lock.json 文件。

6. 删除某些依赖：

   - 执行 npm uninstall 命令，验证没问题后，提交新的 package.json、package-lock.json 文件；
   - 或者手动操作 package.json，删除依赖，执行 npm install 命令，验证没问题后，提交新的 package.json、package-lock.json 文件。

7. 任何团队成员提交 package.json、package-lock.json 更新后，其他成员应该拉取代码后，执行 npm install 更新依赖。

8. 任何团队成员提交 package.json、package-lock.json 更新后，其他成员应该拉取代码后，执行 npm install 更新依赖。

9. 如果 package-lock.json 出现冲突或问题，建议将本地的 package-lock.json 文件删除，引入远程的 package-lock.json 文件和 package.json，再执行 npm install 命令。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[::first-letter (:first-letter) - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-letter)

[text-transform - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-transform)

[npm-ci | npm Docs](https://docs.npmjs.com/cli/v8/commands/npm-ci)

[语义化版本 2.0.0 | Semantic Versioning](https://semver.org/lang/zh-CN/)
