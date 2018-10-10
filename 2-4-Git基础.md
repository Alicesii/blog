---
title: 2-4-Git基础
comments: true
date: 2018-07-27 00:10:35
categories: 博客列表
tags: Git
about:

---

## 4.撤销操作

在任何一个阶段，你都有可能想要撤消某些操作，但是有些撤销操作是不可逆的。

有时我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。这时，可以运行带有`--amend`选项的提交命令尝试重新提交。

```
$ git commit --amend
```

该命令会将暂存区中的文件提交。如果自上次提交以来你还未做任何修改，那么快照会保持不变，而你所修改的只是提交信息。

举例：你提交后发现忘记了暂存某些需要的修改，这时，就可以运用该命令了。

```
$ git commit -m 'initial commit'
$ git add forgotten_file.md
$ git commit --amend
```

最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

### 4.1.取消暂存的文件

如果你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却输入了`git add *`同时暂存了它们两个。如何只取消暂存两个中的一个呢？

```
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   one.html
        new file:   two.html
```

上述代码已经提示的很清楚，使用`"git reset HEAD <file>..."`来取消暂存文件。

```
$ git reset HEAD one.html
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   two.html
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

         new file: one.html
```

我们可以看到one.html文件已经被修改为未暂存的状态。

### 4.2.撤销对文件的修改

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

         new file: one.html
```

如何撤销对文件的修改，也就是将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子），我们还需要依照提示来做：使用`"git checkout -- <file>..."`来撤销对文件的修改。

```
$ git checkout -- one.html
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   one.html
        new file:   two.html
```

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