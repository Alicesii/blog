---
title: 2-1-Git基础
comments: true
date: 2018-07-24 10:16:20
categories: 博客列表
tags: Git
about:

---

## 1.获取Git仓库

有两种方法可以获取到Git项目仓库。第一种是在现有项目下导入所有文件到Git；第二种是从一个服务器克隆一个现有的Git仓库。

### 1.1.在现有目录中初始化仓库

在现有目录点击右键，就可以看到Git Bash的快捷键方式，

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_3.png)

`git init`命令将创建一个.git的子目录，这个子目录含有初始化的Git仓库中所有的必须文件，这些文件是Git仓库的骨干。但是，仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。

如果是在一个已经存在文件的文件夹中初始化Git仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。可以通过`git add`命令来实现对文件的跟踪，然后执行`git commit`提交。

```
git add *

git commit -m "first-commit"
```

现在，已经得到了一个实际维护(或者说是跟踪)着若干个文件的Git仓库。

### 1.2.克隆现有的仓库

`git clone`命令：获取一份已经存在了的Git仓库的拷贝。

Git克隆的是该Git仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。当你执行`git clone`命令的时候，默认配置下远程Git仓库中的每一个文件的每一个版本都将被拉取下来。

```
git clone [url]

git clone https://github.com/Alicesii/blog
或
git clone https://github.com/Alicesii/blog myblog   //自定义本地仓库的名字myblog
```

在该目录下初始化一个`.git`文件夹，从远程仓库拉取下所有数据放入`.git`文件夹，然后从中读取最新版本的文件的拷贝。

Git支持多种数据传输协议，可以使用的是`https://`协议，也可以使用`git://`协议或者使用SSH传输协议。

## 2.记录每次更新到仓库

假如我们有一个项目的Git仓库，并从这个仓库中取出所有文件的工作拷贝。接下来，对这些文件做些修改，在完成了一个阶段的目标之后，提交本次更新到仓库。

工作目录下的每一个文件都只存在着这两种状态：已跟踪或未跟踪。

已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。

工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。

初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

编辑过某些文件之后，由于自上次提交后对它们做了修改，Git将它们标记为已修改文件。我们可以将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。使用Git时文件的生命周期如下：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1268239/o_7.png)

文件的状态变化周期

### 2.1.检查当前文件状态

查看文件处于哪些状态，使用`git status`命令。

如果在克隆仓库后立即执行此命令，会是这样的输出：

```
$ git status
On branch master
nothing added to commit, working directory clean
```

说明现在的工作目录十分干净。也就是说，所有已跟踪文件在上次提交后都未被更改过。并且当前目录下没有出现任何处于未跟踪状态的新文件，否则Git会在这里列出来。该命令还显示了当前所在分支，并且告诉你这个分支同远程服务器上对应的分支没有偏离。

我们可以在项目下创建一个README.MD的文件，如果之前不存在这个文件，使用`git status`命令，将看到一个新的未跟踪文件。

```
$ echo 'css garden' >README.md
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

         README.md
```

在状态报告中可以看到新建的README.MD文件出现在`Untracked files`下。未跟踪的文件意味着Git在之前的快照(提交)没有这些文件，Git不会自动将之纳入跟踪范围。

### 2.2.跟踪新文件

开始跟踪一个文件，使用`git add`命令。

```
$ git add README.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README.md
```

READMD文件文件已被跟踪，并处于暂存状态。

在`Changes to be committed`下的所有文件都是已暂存状态，如果此时提交，那么该文件此时此刻的版本江北存留在历史记录中。

### 2.3.暂存已修改文件

修改一个已被跟踪的文件，然后运行`git status`命令，

```
$ git status
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   five.html
```

文件five.html出现在`Changes not staged for commit`下，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，又需要使用git add命令。

git add命令有多种作用：

* 跟踪新文件

* 把已跟踪的文件放到暂存区

* 合并时把有冲突的文件标记为以解决状态

* "添加内容到下一次提交中"而不是"将一个文件添加到项目中"

```
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README.md
        modified:   five.html
```

两个文件都已暂存，下次提交时就会一并记录到仓库。

如果需要在修改five.html文件，我们可以再看一下暂存区的状态：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README.md
        modified:   five.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   five.html
```

我们可以发现five.html文件同时出现在暂存区和非暂存区，这种情况是不可能发生的，实际上Git只暂存了你运行`git add`命令时的那个版本，如果你现在提交，five.html的版本是你最后一次运行`git add`命令时的那个版本，而不是你运行`git commit`时，在工作目录中的当前版本。因此，运行了`git add`之后又作了修订的文件，需要重新运行git add把最新版本重新暂存起来。

```
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   README.md
        modified:   five.html
```

### 2.4.状态简览

git status命令的输出十分详细，但又有些繁琐，可以使用`git status -s`命令，得到一种更为紧凑的格式输出。

```
$ git status -s
 M  README.md
MM  five.html
A  five.txt
?? .project
```

新添加的未跟踪文件前面有`??`标记，新添加到暂存区中的文件前面有`A`标记，修改过的文件前面有`M`标记。你可能注意到了`M`有两个可以出现的位置，出现在右边的 `M`表示该文文件被修改了但是还没放入暂存区，出现在靠左边的`M`表示该文件被修改了并放入了暂存区。




