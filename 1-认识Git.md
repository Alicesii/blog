---
title: 认识Git
comments: true
date: 2018-07-22 07:40:29
categories: 博客列表
tags: Git
about:

---

## 1.认识Git

Git是什么？用来做什么?

Git用来进行版本控制......

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。就好像在PS中修改图片时，你会建立多个图层。你可以很方便的找到任何一个图层。

* 本地版本控制系统

采用某种简单的数据库来记录文件的历次更新差异。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_1.png)

* 集中式版本控制系统

在实际的开发中，一个项目通常是由多人协作共同完成的，如何让在不同系统上的开发者协同工作成了亟待解决的问题，集中式版本控制系统便应运而生了。它通过单一的集中管理的服务器，保存所有文件的修订版本，协同工作的开发者都通过客户端连到这台服务器，取出最新的文件或者提交更新。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_2.png)

这种方式很好解决了多人协同开发的问题，但是也存在着很大的弊端，若中央服务器的单点故障。如果宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。 如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，毫无疑问你将丢失所有数据——包括项目的整个变更历史，只剩下人们在各自机器上保留的单独快照。本地版本控制系统也存在类似问题，只要整个项目的历史记录被保存在单一位置，就有丢失所有历史更新记录的风险。

* 分布式版本控制系统(Git是典型的分布式管理系统)

在分布式版本控制系统中，客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_3.png)


分布式版本控制系统，则不需要中央服务器，每个协同开发者都拥有一个完整的版本库，这么一来，任何协同开发者用的服务器发生故障，事后都可以用其它协同开发者本地仓库恢复。由于版本库在本地计算机，也便不再受网络影响了。如果要将本地的修改，推送给其它协同开发者，还需要一台共享服务器，所有开发者通过这台共享服务器提交和更新数据。

## 2.了解Git

### 2.1.直接记录快照，而非差异

Git和其他版本控制系统的主要差别在于Git对待数据的方法，其他大部分系统以文件变更列表的方式存储信息。这类系统将它们保存的信息看作是一组基本文件和每个文件随时间逐步积累的差异。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_4.png)
存储每个文件与初始版本的差异

Git把数据看作是对小型文件系统的一组快照，每次提交更新，或在Git中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引，为了提高效率，如果文件没有修改，Git不再重复存储该文件，而是只保留一个链接指向之前存储的文件，Git对待数据更像是一个**快照流**

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_5.png)
存储项目随时间改变的快照

这是Git与其他版本控制系统的重要区别，Git重新考虑了以前每一代版本控制系统延续下来的诸多方面。Git更像是一个小型文件系统，提供了许多以此为基础构建的工具，而不是一个简单的VCS(版本控制系统)。

### 2.2.近乎所有操作都是本地

在Git中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其他计算机的信息。

举例：要浏览项目的历史，Git不需要外连服务器去获取历史，然后再显示出来，它只需要从本地数据库中读取，就可以看到项目历史。如果你想查看当前版本与一个月前的版本之间的修改，Git会查找到一个月前的文件做一次本地的差异比较，而不是由远程服务器处理或从远程服务器拉回旧版本文件在做来本地处理，这也意味着你离线或者没有VPN时，几乎可以做任何操作。

### 2.3.Git保证完整性

Git中所有数据在存储前都计算校验和，然后以校验和来引用。这意味着不可能在Git不知情时更改任何文件内容或目录内容。如果在传送过程中丢失信息或损坏文件，Git就能发现。

Git用以计算校验和的机制叫做SHA-1散列(哈希)。这是一个由40个十六进制字符(0-9和a-f)组成字符串，基于Git中文件的内容或目录结构计算出来的。SHA-1哈希看起来是这样的：

e65b40b8c5f3d8d69fcf7fe546c43a0d48df2c1d

Git数据库中保存的信息都是以文件内容哈希值来索引，而不是文件名。

### 2.4.Git一般只添加数据

所执行的Git操作，几乎只往Git数据库中增加数据。很难让Git执行任何不可逆操作，或者让它以任何方式清除数据。同别的VCS(版本控制系统)一样，未提交更新时有可能丢失或弄乱修改的内容；但是一旦提交快照到Git中，就难以再丢失数据，特别是如果你定期的推送数据库到其它仓库的话。

### 2.5.三种状态

Git有三种状态，你的文件可能处于其中之一：已提交、已修改、已暂存。

已提交表示数据已经安全的保存在本地数据库中。

已修改表示修改了文件，但还没保存到数据库中。

已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

Git 项目的三个工作区域的概念：Git仓库、工作目录以及暂存区域

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_6.png)

Git仓库目录是Git用来保存项目的元数据和对象数据库的地方。这是Git中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录是对项目的某个版本独立提取出来的内容。这些从Git仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在Git仓库目录中。有时候也被称作`"索引"`，不过一般说法还是叫暂存区域。

基本的Git工作流程如下：

1、在工作目录中修改文件。

2、暂存文件，将文件的快照放入暂存区域。

3、提交更新，找到暂存区域的文件，将快照永久性存储到Git仓库目录。

### 2.6.命令行

Git有多种使用方式。你可以使用原生的命令行模式也可以使用GUI模式，这些GUI软件也能提供多种功能，但是只有在命令行模式下你才能执行Git的所有命令。

## 3.初次运行Git前的配置

### 3.1.用户信息

安装完Git应该做的第一件事就是设置你的用户名称与邮件地址。这样做是非常有必要的，因为每一个Git的提交都会使用这些信息并且它会写入到你的每一次提交中不可更改：

```
$ git config --global user.name "Yang Ying"

$ git config --global user.email 17868812265@163.com
```

如果使用了`--global`选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情，Git都会使用那些信息。当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有`--global`选项的命令来配置。

```
$ git config user.name "Yang Ying"

$ git config user.email 17868812265@163.com
```

### 3.2.文本编辑器

Git操作系统默认的文本编辑器是Vim。

### 3.3.检查配置信息

使用`git config --list`命令可以列出所有Git当时能找到的配置。

```
$ git config --list
core.symlinks=false
core.autocrlf=true
core.fscache=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
help.format=html
http.sslcainfo=F:/Git/mingw64/ssl/certs/ca-bundle.crt
diff.astextplain.textconv=astextplain
rebase.autosquash=true
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
user.name=ying
user.email=1511317497@qq.com
user.password=123456
credential.helper=store
core.autocrlf=false
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
core.hidedotfiles=dotGitOnly
remote.origin.url=git@github.com:255255255255/CSSGarden.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
```

通过输入`git config <key>`:来检查Git的某一项配置

```
$ git config user.name

ying
```

### 3.4.获取帮助

```
git help config
```

Git中几乎所有命令都是在本地执行，不需要请求服务器端的信息。