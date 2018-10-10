---
title: 2-6-Git基础
comments: true
date: 2018-07-29 00:10:35
categories: 博客列表
tags: Git
about:

---

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
