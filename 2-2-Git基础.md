---
title: 2-2-Git基础
comments: true
date: 2018-08-03 22:58:09
categories: 前端
tags: Git
about:

---

### 2.5.忽略文件

一般我们总会有些文件无需放入Git管理，也不希望它们总出现在未跟踪文件列表，这时我们可以创建一个
`.gitignore`的文件，列出要忽略的文件模式。

```
touch .gitignore
*.[oa]
*~
```

第一行告诉Git忽略所有以`.o`或`.a`结尾的文件

第二行告诉Git忽略所有以波浪符`（~）`结尾的文件

文件`.gitignore`的格式规范如下：

* 所有空行或者以`＃`开头的行都会被Git忽略。

* 可以使用标准的glob模式匹配(正则表达式)。

* 匹配模式可以以`（/）`开头防止递归。

* 匹配模式可以以`（/）`结尾指定目录。

* 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号`（!）`。

参考链接：https://github.com/github/gitignore

### 2.6.查看已暂存和未暂存的修改

假如我们再次修改README.md文件后暂存，然后编辑five.html文件后先不暂存，我们可以看下当前的状态：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   five.html
```

如果你想知道具体修改了什么地方，`git status`命令的输出的结果太过模糊，我们可以使用`git diff`命令。通过该命令我们可以知道：

* 当前做的哪些更新还没有暂存？

* 有哪些更新已经暂存起来准备好了下次提交？

```
$ git diff
diff --git a/five.html b/five.html
index 59a5ccd..a4e2f77 100644
--- a/five.html
+++ b/five.html
@@ -55,7 +55,6 @@
<h3>效益</h3>
<p>为什么要参加？为了识别、灵感和资源，我们都可以向人们展示</p></div>
<div class="requirements" id="zen-requirements" role="article">
<h3>要求</h3>
<p>在可能的情况下,实用的而不是2%的浏览公众最新的流血技巧。我们唯一的真正要求是您的验证</p>
```

`git diff`命令比较的是工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容。

我们可以使用`git diff --cached`查看已暂存的将要添加到下次提交里的内容。

```
$ git diff --cached
diff --git a/five.html b/five.html
index e231252..59a5ccd 100644
--- a/five.html
+++ b/five.html
@@ -4,17 +4,14 @@
<head>
<meta charset="utf-8">
<title>CSS禅意花园:CSS设计之美</title>
</head>
<body id="css-zen-garden">
<div class="page-wrapper">
<section class="intro" id="zen-intro">
 <header role="banner">
 <h1>CSS Zen Garden</h1>
@@ -35,7 +32,6 @@
<p>CSS禅园邀请你放松和沉思大师们的重要课程</p>
</div>
</section>
@@ -43,12 +39,10 @@
<div class="participation" id="zen-participation" role="article">
<h3>参与</h3>
<p>您可以按照您希望的任何方式修改样式表</p>
<p>下载示例和
@@ -129,7 +123,6 @@
 </ul>
  </nav>
 </div>
```

### 2.7.提交更新

在提交之前，一定要确认还有没有什么修改过的或新建的文件还没有`git add`过，否则提交的时候不会记录这些还没暂存起来的变化。

```
git commit
```

`git commit`命令会启动文本编辑器以便输入本次提交的说明。

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
# new file: README.md
# modified: five.html
#
~
~
~
```

我们可以看出，默认的提交消息包含最后一次运行`git status`的输出，全都放在注释行中。

我们大多数的用法是在commit命令后面添加-m选项，将提交信息与命令放在同一行。

```
$ git commit -m 'first-commit'
[master 4f38a76] first-commit
 3 files changed, 3 insertions(+), 10 deletions(-)
 create mode 100644 README.md
 create mode 100644 five.html

```

至此，我们已经完成了第一个提交，提交之后会显示当前在哪个分支提交的，本次提交的完整SHA-1校验和是4f38a76，以及在本次提交中，有多少文件修订过，多少行添加和修改过。

### 2.8.跳过使用暂存区域

我们可以将`git add`和`git commit`命令的功能合并成为一个命令，那就是`git commit -a`。

 该命令就是跳过使用暂存区域，也就是说Git会自动把所有已跟踪过的文件暂存起来一并提交。这个基本不太使用，大家了解一下就好。

### 2.9.移除文件

`git rm`命令:从已跟踪文件清单中移除某个文件，更明确地说是从暂存区域移除，也就是说从暂存区域并连带从工作目录中删除指定文件，这样以后就不会出现在未跟踪文件清单中了。

如果只是从工作目录删除文件，运行`git status`时所删除的文件就会在`"Changes not staged for commit"`部分(未暂存清单)中看到：

```
$ rm README.md
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    README.md
```

我们可以使用`git rm`命令删除文件看下与普通的删除文件的区别：

```
$ git rm README.md
rm README.md
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:   README.md
```

从暂存区域并连带从工作目录中删除了README.md文件，也就意味着该文件就不在纳入版本管理了。

如果我们想把文件从Git仓库中删除(从暂存区域移除)，但仍然希望保留在当前工作目录中。也就是说，想让文件保留在磁盘，但是并不想让Git继续跟踪(.gitignore文件的作用)。可以在上添加`git rm`命令上添加`--cached`选项：


```
$ git rm --cached README.md
```