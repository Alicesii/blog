---
title: 3-3-Git分支
comments: true
date: 2018-08-05 07:08:59
categories: 博客列表
tags: Git
about:

---

## 3.分支管理

`git branch`命令在不加任何参数运行时，会得到当前所有分值的一个列表。

```
$ git branch
  iss16
  * master
  feature
```

`*`字符：代表现在正处于哪一个分支(master分支)，也就是当前HEAD指针所指向的分支。如果在这时提交，master分支会随着新的工作向前移动。

`git branch -v`命令：查看每一个分支的最后一次提交。

```
$ git branch -v
 iss16 93b412c new branch
 * master  0f704f0 master
 feature bb5c431 anther brabch
```

`--merged`选项过滤列表中已经合并到当前分支的分支。

```
$ git branch --merged
  iss16
* master
```

在该列表中分支名字前没有`*`号的分支可以使用`git branch -d`命令删除。

`--no-merged`选项过滤列表中尚未合并到当前分支的分支。

```
$ git branch --no-merged
  feature
```

使用`git branch -d`命令删除feature命令时会失败。

```
$ git branch -d feature
error: The branch 'feature' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature'.
```

可以使用`'git branch -D'`命名删除feature分支，但是会丢掉在该分支上的工作。

## 4.分支开发工作流

### 4.1.长期分支(master分支)

在整个项目开发周期的不同阶段，可以拥有多个开放的分支，可以定期地把某些特性分支合并入其他分支中。

一般情况下，在master分支上保留完全稳定的代码，有可能仅是已经发布或即将发布的代码，可能有一些名字为develop或next的平行分支，被用来做后续开发或者测试稳定性。

develop或next分支不需要保持绝对稳定，一旦达到稳定状态，就需要被合并到master分支。

在确保已完成的特性分支(短期分支[iss16])能够通过所有测试，并且不会引入更多bug之后，就可以合并入主干分支，等待下一次的发布。

稳定分支的指针总是在提交历史中落后一大截，前沿分支的指针比较靠前。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_25.png)
渐进稳定分支的线形图。

就像流水线一样，经过测试的提交会被提交到更加稳定的流水线。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_26.png)
渐进稳定分支的流水线视图

### 4.2.特性分支

特性分支是一种短期分支，它被用来实现单一特性或其相关工作。

在特性分支(iss16和hotfix分支)中提交一些更新，并且把它们合并到主干分支之后，又删除了特性分支。这样做能使你快速并且完整地进行上下文切换，因为你的工作被分散到不同的流水线中，在不同的流水线中每个分支都仅与其目标特性相关。

举例：

你在master分支上工作到C1，这时为了解决问题`#18`而新建iss91分支，在iss91分支上工作到C4，对于问题`#18`你又有了新的想法，所有你再新建一个iss91v2分支试图用另一种方法解决问题#18，接着你回到master分支工作了一会儿，你又冒出了一个不太确定的想法，于是你在C10的时候新建一个dumbidea分支，并在上面解决该问题。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_27.png)
拥有多个特性分支的提交历史

假如你决定使用iss91v2分支和dumbidea分支中的方案的其中一个，就可以丢掉和iss91分支(丢弃C5和C6提交)，然后把另外两个分支合并入主干分支。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_28.png)
合并dumbidea分支和iss91v2分支

## 5.远程分支

远程跟踪分支是远程分支状态的引用，它们是不能移动的本地分支，当不论做任何操作时，它们会自动移动。

命名形式：(remote)/(branch)

举例：查看最后一次与远程仓库origin通信时master分支的状态，可以查看origin/master分支。

假设从Git服务器git.ying.com上克隆一个项目，Git的pull命令会为你自动将其命名为origin，拉取它的所有数据，创建一个指向它的master分支的指针，并且在本地将其命名为origin/master。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_29.png)
克隆之后的服务器与本地仓库

如果你在本地的master分支做了一些工作，在同一时间，其他人推送提交到git.ying.com服务器并更新了它的master分支，这时会发生两种状况。

* 第一种情况：与origin服务器连接

提交历史将向不同的方向前进。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_30.png)

* 第二种情况：没有与origin服务器连接

origin/master指针不会移动

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_31.png)
本地与远程的工作分叉

`git pull origin`命令：向远程仓库同步你的工作。

`git pull origin`命令：查找`"origin"`是哪一个服务器(git.ying.com)，从中抓取本地没有的数据，并且更新本地数据库，移动origin/master指针指向新的、更新后的位置。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_32.png)
`git pull`命令更新远程仓库地引用

举例：

假设存在另一个内部的Git服务器，用于开发新的项目，这个服务器位于`git.my_ying.com`，我们可以添加另一个新的远程仓库引用到当前新的项目。

`git remote add`命令：添加一个新的远程仓库引用到当前的项目，远程仓库的命名为`my_ying`。


![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_33.png)
添加另一个远程仓库


运行`git pull origin`命令来抓取远程仓库`my_ying`而本地没有的数据，这台服务器上现有的数据是origin服务器上的一个子集，所以Git并不会抓取数据而是会设置远程跟踪分支`my_ying/master`指向`my_ying`的master分支。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_34.png)
远程跟踪分支`my_ying/master`


