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


