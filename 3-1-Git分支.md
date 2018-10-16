---
title: 3-1-Git分支
comments: true
date: 2018-08-09 11:20:19
categories: 前端
tags: Git
about:

---

> 使用分支意味着你可以把你的工作从开发主线上分离出来，以免影响开发主线。合理的利用分支，将会提高你的开发效率。

## 1.分支简介

Git保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照。

在进行提交操作时，Git会保存一个提交对象，该提交对象会包含一个指向暂存内容快照的指针，还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象，而由多个分支合并产生的提交对象有多个父对象。

举例：假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。暂存操作会为每一个文件计算校验和(SHA-1哈希算法)，然后会把当前版本的文件快照保存到Git仓库中，最终将校验和加入到暂存区域等待提交。

```
$ git add A.md B.md C.md
$ git commit -m 'my project'
[master (root-commit) 6beec10] my project
 3 files changed, 194 insertions(+)
 create mode 100644 A.md
 create mode 100644 READ.md
 create mode 100644 README.md
$ git log
commit 6beec10a754d1614768a055bff34aa007910b30e
Author: ying <1511317497@qq.com>
Date:   Thu Aug 2 22:23:11 2018 +0800

    my project
```

当使用`git commit`进行提交操作时，Git会先计算每一个子目录(项目根目录)的校验和，然后在Git仓库中这些校验和保存为树对象。随后，Git便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象(项目根目录)的指针。

也就是说，Git仓库中有五个对象，三个blob对象(保存着文件快照)、一个树对象(记录着目录结构和blob对象索引)以及一个提交对象(包含着指向树对象的指针和所有提交信息)。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_8.png)
首次提交对象及其树结构

做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象(父对象)的指针。

```
$ git log
commit b4fa11682c5a00cacdbc103bfbd6795a6dd900b2
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:37:49 2018 +0800

    second-Git

commit 9c36a3dba4a14c96b650555658a01f211c9274bf
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:36:55 2018 +0800

    first-Git

commit 6beec10a754d1614768a055bff34aa007910b30e
Author: ying <1511317497@qq.com>
Date:   Thu Aug 2 22:23:11 2018 +0800

    my project
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_9.png)
提交对象及其父对象

Git的分支，其实本质上仅仅是指向提交对象的可变指针。Git的默认分支名字是master。在多次提交操作之后，你其实已经有一个指向最后那个提交对象的master分支，它会在每次的提交操作中自动向前移动。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_10.png)
分支及其提交历史

### 1.1.分支创建

`git branch`命令：创建新的分支，创建了一个可以移动的新的指针。

```
git branch feature
```

也就是在当前所在的提交对象上创建一个指针

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_11.png)
两个指向相同提交历史的分支

是不是有疑问，Git怎么知道当前在哪一个分支上呢？

在Git中，它有一个名为HEAD的特殊指针，指向当前所在的本地分支。`git branch`命令仅仅创建一个新分支，并不会自动切换到新分支，现在仍然在master分支。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_12.png)
HEAD指向当前所在的分支

使用`git log`命令查看各个分支当前所指的对象，需要使用--decorate参数。

```
$ git log --pretty=oneline --decorate
b4fa11682c5a00cacdbc103bfbd6795a6dd900b2 (HEAD -> master, feature) second-Git
9c36a3dba4a14c96b650555658a01f211c9274bf first-Git
6beec10a754d1614768a055bff34aa007910b30e my project
```

我们可以看到，当前`"master"`和`"feature"`分支均指向校验和以`b4fa116`开头的提交对象。

### 1.2.分支切换

`git checkout`命令：切换到一个已存在的分支。

```
$ git checkout feature
Switched to branch 'feature'
```

现在Head就指向feature分支了。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_13.png)
HEAD指向当前所在的分支

利用分支的形式有什么优点呢，我们可以来看一下：

```
$ git add *
$ git commit -m 'anther brabch'
[feature bb5c431] anther brabch
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git log
commit bb5c431f59dd83b3ffcefa587f2959eba5c3e60d
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 19:11:07 2018 +0800

    anther branch

commit b4fa11682c5a00cacdbc103bfbd6795a6dd900b2
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:37:49 2018 +0800

    second-Git

commit 9c36a3dba4a14c96b650555658a01f211c9274bf
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:36:55 2018 +0800

    first-Git

commit 6beec10a754d1614768a055bff34aa007910b30e
Author: ying <1511317497@qq.com>
Date:   Thu Aug 2 22:23:11 2018 +0800

    my project
$ git log --pretty=oneline --decorate
bb5c431f59dd83b3ffcefa587f2959eba5c3e60d (HEAD -> feature) anther branch
b4fa11682c5a00cacdbc103bfbd6795a6dd900b2 (master) second-Git
9c36a3dba4a14c96b650555658a01f211c9274bf first-Git
6beec10a754d1614768a055bff34aa007910b30e my project
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_14.png)
HEAD分支随着提交操作自动向前移动

从图中可以，feature分支向前移动了，但master分支却没有，我们可以切换到master分支。

```
$ git checkout master
Switched to branch 'master'
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_15.png)
HEAD指针随着分支的变化移动

`git checkout`命令有两个功能：

* 一是使HEAD指针指回master分支

* 二是将工作目录恢复成master分支所指向的快照内容，也就是说，如果现在修改的话，项目将使于一个较旧的版本，本质上来说，是忽略eature分支所做的修改，以便于向另一个方向进行开发。

注意：分支切换会改变你工作目录中的文件

在切换分支时，如果切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的状态。

```
$ git add *

dell@yang MINGW64 /e/维护/A/算法的日常/链表 (master)
$ git commit -m 'master'
[master 0f704f0] master
 3 files changed, 3 insertions(+), 3 deletions(-)

dell@yang MINGW64 /e/维护/A/算法的日常/链表 (master)
$ git log
commit 0f704f0df86d9e044906c557be9d5976e91f1076
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 20:42:13 2018 +0800

    master

commit b4fa11682c5a00cacdbc103bfbd6795a6dd900b2
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:37:49 2018 +0800

    second-Git

commit 9c36a3dba4a14c96b650555658a01f211c9274bf
Author: ying <1511317497@qq.com>
Date:   Fri Aug 3 15:36:55 2018 +0800

    first-Git

commit 6beec10a754d1614768a055bff34aa007910b30e
Author: ying <1511317497@qq.com>
Date:   Thu Aug 2 22:23:11 2018 +0800

    my project
```

这时，项目的提交历史已经产生了分支，因为刚才创建了一个新分支，并切换过去进行了一些工作，随后又切换回master分支进行了另外一些工作。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_16.png)
项目分叉历史

使用`git log`命令查看各个分支当前所指的对象以及分支的历史，利用--oneline、--decorate、--graph、--all参数，它会输出你的提交历史、各个分支的指向以及项目的分支分叉情况。

```
$ git log --pretty=oneline --decorate --graph --all
* 0f704f0df86d9e044906c557be9d5976e91f1076 (HEAD -> master) master
| * bb5c431f59dd83b3ffcefa587f2959eba5c3e60d (feature) anther brabch
|/
* b4fa11682c5a00cacdbc103bfbd6795a6dd900b2 second-Git
* 9c36a3dba4a14c96b650555658a01f211c9274bf first-Git
* 6beec10a754d1614768a055bff34aa007910b30e my project
```
