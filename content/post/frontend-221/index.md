---
title: '毎日のフロントエンド 221'
date: 2022-05-01T22:26:24+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### wbr 和 br 标签的区别

- `wbr` (word break opportunity)规定在文本的何处进行添加换行符, wbr 是用来实现连续英文和数字的精准换行的，具体效果是如果宽度足够就不换行，如果宽度不足则在 wbr 元素在的位置进行换行。
  - 它创建了一个带有换行特性的宽度为 0px 的空格。这个空格的 Unicode 编码是 U+200B，因此这个标签也可以替换为 `&#x200b`
- `br`是整行换行, 表示必须换行

## CSS

### css 实现左右拉伸拖动

```css
.class {
  resize: horizontal;
}
```

## Tips

### GIT

#### 场景一：添加到一个新的修改，到最新的 commit 中

```bash
# 添加这个对应的小的修改
git add .

# 提交并且使用两个特殊的参数
# --amend 修改最新的一次 commit，将现在 staged change 直接添加到上一次 commit 去，不生成新的 commit
# --no-edit amend 的提交不修改提交信息
git commit --amend --no-edit

# 修改上一次commit的message
git commit --amend
```

#### 场景二：Git 时光机： `reflog + reset`

```bash
# 显示你的 git 操作记录
# git log  有很大区别，它不仅仅是提交的记录，还有其他git的操作记录
git reflog
# 会看到一系列的操作记录

# 7b6e4f8 HEAD@{0}
# e7d2c90 HEAD@{1} ...
git reset HEAD@{index} # 这个时候你就会回到对应的状态
```

- `git reset --hard` 工作区和暂存区以及本地仓库全受影响
- `git reset` 默认，重置本地仓库的同时，还会修改暂存区的内容（工作区修改还在）
- `git reset --sort` 只修改 Head 指针，即**只重置本地仓库**的记录 (工作区和暂存区的修改都在)

---

#### 场景三：Git 后悔药： `git rebase -i <commit-hash>` or:

- 将某一个 commit 中的文件，修改为：这个 commit 的前一个 commit 中的状态：

```bash
git log
# f9f6f3b commit 3
# 2feb45f commit 2
# 07a3cb6 commit 1
# commit2 出问题了，我们要恢复某个文件在 commit 1中的内容，先看看它修改了什么
git show 2feb45f
test.txt
-hello there updated
+hello there deleted
# 可以发现它将 test.txt 从 updated 换成了 deleted，我们希望恢复，使用一条命令
git checkout 07a3cb6[之前提交的hash，也就是 commit1 的hash] -- test.txt[文件的路径]
# 这个时候你就会发现 test.txt 恢复到了 hello there updated
# 🎉 一条命令搞定
```

- `git revert`

```bash
# f9f6f3b commit 3
# 2feb45f commit 2
# 07a3cb6 commit 1
# commit2 出问题了，我们要一次性删除掉对应的提交,找到上面第二条对应的 hash 值
git revert 2feb45f[第二条对应的 hash]
# 这个时候它会创建一个新的 commit，并且弹出提交的对话框，修改保存就可以了。
```

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [resize - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/resize)

- [Oh Shit, Git!?!](https://ohshitgit.com/)
