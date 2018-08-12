---
title: 2-3-Git基础
comments: true
date: 2018-07-28 17:30:29
categories: 博客列表
tags: Git
about:

---

## 5.远程仓库的使用

远程仓库是指托管在网络中的你的项目的版本库，你可以同时拥有多个远程仓库，通常有些仓库只读，有些既可以读也可以写。管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义它们是否被跟踪等等。

### 5.1.查看远程仓库

`git remote`命令：列出你指定的每一个远程服务器的简写。

```
$ git pull origin master
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
Unpacking objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
From github.com:Alicesii/javascript-Object
 * branch            master     -> FETCH_HEAD
   6d95c79..d08bd5e  master     -> origin/master
Updating 6d95c79..d08bd5e
Fast-forward
 README.md | 59 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 59 insertions(+)
 create mode 100644 README.md
$ git remote
origin
```

origin是Git克隆的仓库服务器的默认名称。

指定选项-v，会显示需要读写远程仓库使用的Git保存的简写与其对应的URL。

```
$ git remote -v
origin  git@github.com:Alicesii/javascript-Object.git (fetch)
origin  git@github.com:Alicesii/javascript-Object.git (push)
```

多人合作完成的项目，肯定拥有多个远程仓库，该命令会将它们全部列出。

```
$ git remote -v
bakkdoor https://github.com/bakkdoor/grit (fetch)
bakkdoor https://github.com/bakkdoor/grit (push)
cho45 https://github.com/cho45/grit (fetch)
cho45 https://github.com/cho45/grit (push)
defunkt https://github.com/defunkt/grit (fetch)
defunkt https://github.com/defunkt/grit (push)
koke git://github.com/koke/grit.git (fetch)
koke git://github.com/koke/grit.git (push)
origin git@github.com:mojombo/grit.git (fetch)
origin git@github.com:mojombo/grit.git (push)
```

### 5.2.添加远程仓库

`git remote add <shortname> <url>`：添加一个新的远程Git仓库。

```
$ git remote
origin
$ git remote add Ades https://github.com/Alicesii/blog
$ git remote -v
Ades    https://github.com/Alicesii/blog (fetch)
Ades    https://github.com/Alicesii/blog (push)
origin  git@github.com:Alicesii/javascript-Object.git (fetch)
origin  git@github.com:Alicesii/javascript-Object.git (push)
```

可以在命令行中使用字符串Ades来代替整个URL。例如，如果你想拉取blog的仓库但是没有它的URL信息，就可以运行`git pull Ades`。

```
$ git pull Ades
remote: Counting objects: 119, done.
remote: Compressing objects: 100% (113/113), done.
Receiving objects:  7remote: Total 119 (delta 34), reused 72 (delta 6), pack-reused 0
Receiving objects: 100% (119/119), 284.63 KiB | 164.00 KiB/s, done.
Resolving deltas: 100% (34/34), done.
From https://github.com/Alicesii/blog
 * [new branch]      master     -> Ades/master
```

### 5.3.从远程仓库中抓取与拉取

```
$ git pull [remote-name]
```

该命令会访问远程仓库，从中拉取所有本地仓库中没有的数据。执行完成后，你将会拥有该远程仓库中所有分支的引用，可以随时合并或查看。

git pull命令会将数据拉取到你的本地仓库，它并不会自动合并或修改你当前的工作。

### 5.4.推送到远程仓库

`git push [remote-name] [branch-name]`命令：将项目推送到远程服务器。

我们最常使用的是将master分支推送到origin服务器上。

```
$ git push origin master
```

只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 当你和其他人在同一时间克隆，他们先推送到服务器然后你再推送，你的推送就会毫无疑问地被拒绝。你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

### 5.5.查看远程仓库

`git remote show [remote-name]`命令：查看某一个远程仓库的更多信息。

```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:Alicesii/Git.git
  Push  URL: git@github.com:Alicesii/Git.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
该命令会列出远程仓库的URL与跟踪分支的信息。

### 5.6.远程仓库的移除与重命名

`git remote rename`命令：重命名远程仓库的简写名称。

```
$ git remote add Ades https://github.com/Alicesii/blog
$ git remote rename Ades Bdes
$ git remote
Bdes
origin
```

`git remote rm`命令：移除一个远程仓库

```
$ git remote rm Bdes
$ git remote
origin
```

## 6.打标签

Git可以给历史中的某一个提交打上标签，作为重要的标志。常用这个功能来标记发布节点。

### 6.1.列出标签

`git tag`命令：列出Git中已有的标签。

```
$ git tag
v1.0
v1.1
```
以字母顺序列出标签，但是它们出现的顺序并不重要。

可以使用特定的模式查找标签：

```
$ git tag -l 'v1.8.5*'
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5
```

### 6.2.创建标签

Git使用两种类型的标签：轻量标签和附注标签。

一个轻量标签像一个不会改变的分支，它只是一个特定提交的引用。

附注标签是存储在Git数据库中的一个完整对象，它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间;一个标签信息;并且可以使用GPG签名与验证。

一般情况下都会创建附注标签，这样你可以拥有以上所有的详细信息;但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，这时可以选择轻量标签。

注：GPG它是目前最流行、最好用的加密工具之一。

### 6.3.附注标签

`git tag -a`命令：创建一个附注标签。

```
$ git tag -a v1.1 -m 'my version 1.1'
$ git tag
v1.0
v1.1
```

-m选项指定了一条将会存储在标签中的信息。如果没有为附注标签指定一条信息，Git会运行编辑器要求你输入信息。

`git show`命令：看到标签信息与对应的提交信息。

```
$ git show v1.0
tag v1.0
Tagger: ying <1511317497@qq.com>
Date:   Wed Aug 1 23:12:00 2018 +0800

my version 1.0

commit 474f2b97636314959f312fce6ed9202260363877
Author: ying <1511317497@qq.com>
Date:   Tue Jul 31 16:38:49 2018 +0800

    Git-first

diff --git "a/README.md" "b/README.md"
index a548fa5..3fafe74 100644
--- "a/README.md"
+++ "b/README.md"
@@ -377,7 +377,7 @@ Changes to be committed:
```

输出显示了打标签者的信息、打标签的日期信息、附注信息，然后显示具体的提交信息。

### 6.4.轻量标签

轻量标签本质上是将提交校验和存储到一个文件中，没有保存任何其他信息。

创建轻量标签，不需要使用`-a`、`-s`或`-m`选项，只需要提供标签名字：

```
$ git tag v1.4
$ git tag
v1.0
v1.1
v1.2
v1.3
v1.4
v1.5
```

运行`git show`命令，只会显示出提交信息，不会看到额外的标签信息。

```
$ git show v1.4
commit 474f2b97636314959f312fce6ed9202260363877
Author: ying <1511317497@qq.com>
Date:   Tue Jul 31 16:38:49 2018 +0800

    Git-first

diff --git "a/README.md" "b/README.md"
index a548fa5..3fafe74 100644
--- "a/README.md"
+++ "b/README.md"
@@ -377,7 +377,7 @@ Changes to be committed:
```

### 6.5.后期打标签

假如提交历史如下：

```
$ git log --pretty=oneline
474f2b97636314959f312fce6ed9202260363877 Git-first
7022a109f04ab32f38a7e1bf987e77abb52818fb second-Git
9358d279319fa10a7018e9aa2320259e417c6eb5 first-Git
```

如果在v1.3.1时忘记给项目打标签，也就是在`second-Git`提交，其实你可以在之后补上标签。

需要在命令的末尾指定提交的校验和(或者部分校验和)：

```
$ git tag -a v1.3.1 7022a109f0
$ git tag
$ git tag
v1.0
v1.1
v1.2
v1.3
v1.3.1
v1.4
v1.5

$ git show v1.3.1
tag v1.3.1
Tagger: ying <1511317497@qq.com>
Date:   Wed Aug 2 10:58:01 2018 +0800

my version 1.3.1
commit 474f2b97636314959f312fce6ed9202260363877
Author: ying <1511317497@qq.com>
Date:   Tue Jul 31 16:38:49 2018 +0800

    second-Git

diff --git "a/README.md" "b/README.md"
index a548fa5..3fafe74 100644
--- "a/README.md"
+++ "b/README.md"
@@ -377,7 +377,7 @@ Changes to be committed:
```

### 6.6.共享标签

默认情况下，`git push`命令并不会传送标签到远程仓库服务器上，在创建完标签后你必须显示地推送标签到共享服务器上。

git push origin [tagname]命令

```
$ git push origin v1.0
Counting objects: 1, done.
Writing objects: 100% (1/1), 155 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:Alicesii/Git.git
 * [new tag]         v1.0 -> v1.0
```

带`--tags`选项的`git push`命令，一次性可以推送很多标签，这将会把所有不在远程仓库服务器上的标签全部传送到那里。

```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 155 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:Alicesii/Git.git
 * [new tag]         v1.1 -> v1.1
 * [new tag]         v1.3 -> v1.3
 * [new tag]         v1.3.1 -> v1.3.1
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4 -> v1.5
```

这时，当其他人从仓库中克隆或拉取，它们也能得到你所设置的标签。
