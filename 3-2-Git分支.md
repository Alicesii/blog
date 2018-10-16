---
title: 3-2-Git分支
comments: true
date: 2018-08-11 19:00:39
categories: 前端
tags: Git
about:

---

## 2.分支的新建与合并

分支新建与分支合并的场景(工作流)：

* 1、开发某个网站

* 2、为实现某个新的需求，创建一个分支

* 3、在这个分支上开展工作。

此时，你突然接到一个电话说有个很严重的问题需要紧急修补，你将按照如下方式来处理：

* 1、切换到你的线上分支

* 2、为这个紧急任务新建一个分支，并在其中修复它。

* 3、在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。

* 4、切换回你最初工作的分支上，继续工作。

### 2.1.新建分支

假设存在一个正在开发过程中的项目，并且已经有了一些提交。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_17.png)
一个简单提交历史

如果你需要解决编号为#16的需求问题，你需要创建一个新的分支，然后切换到该分支上。

简写方式：

```
$ git checkout -b iss16
Switched to a new branch 'iss16'

```

非简写方式：

```
$ git branch iss16
$ git checkout iss16
Switched to branch 'iss16'
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_18.png)
创建一个新分支指针

现在可以开始在iss16分支上解决编号#16的需求，在此过程中，你做了一些提交，所以iss16分支在不断的向前推进，此时HEAD指针指向了iss16分支上。

```
$ git add *
$ git commit -a -m 'new branch'

```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_19.png)
iss16分支随着工作的进展向前推进

当你接到电话时，有个紧急问题等待你来解决，在Git下，不需要把这个紧急问题和iss16的修改混在一起，也不需要还原#16问题的修改，然后再添加关于这个紧急问题的修改，最后将这个修改提交到线上分支。你所需要做的仅仅是切回到master分支。

在切回到master分支之前，必须保证所有的工作目录和暂存区中的修改的文件是被提交的状态。

```
$ git status
On branch iss16
nothing to commit, working directory clean
$ git checkout master
Switched to branch 'master'
```

此时，你的工作目录和在开始处理#16需求之前一模一样，现在可以开始修复紧急问题了。

你可以建立一个针对该紧急问题的分支(hotfix branch)，在该分支上解决该紧急问题。

```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim README.md
$ git add *
$ git commit -a -m 'fixed hotfix branch'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```


![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_20.png)
基于master分支的紧急问题分支hotfix branch

注意：当切换分支的时候，Git会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。Git会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。

当修改完成这个紧急问题后，使用`git merge`命令，将hotfix分支合并回master分支。

```
$ git checkout master
Switched to branch 'master'
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```


![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_21.png)
master被快进到hotfix

`Fast-forward`的解释：

Fast-forward表示`"快进"`，由于当前master分支所指向的提交是你当前提交的的直接上游，所以Git只是简单的将指针向前移动。也就是说，当你试图合并两个分支时，如果顺着一个分支走下去能够到达另一个分支，那么Git在合并两者的时候，只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有发生冲突。

当解决完该紧急问题之后，你准备继续处理#16的需求，但是你应该先删除hotfix分支，因为已经不再需要它了，master分支已经指向了同一个位置。

简写方式：

```
$ git checkout -d hotfix
Deleted branch hotfix
```

非简写方式：

```
$ git checkout hotfix
$ git delete hotfix
Deleted branch hotfix
```

这时，可以继续处理#16的需求了。

```
$ git checkout iss16
Switched to branch "iss16"
$ vim README.md
$ git add *
$ git commit -a -m 'fixed iss16 branch'
[iss16 ad82d7a] fixed the broken email address
 1 file changed, 2 insertions(+)
```


![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_22.png)
继续在iss16分支上工作

在hotfix分支上所做的工作并没有包含到iss16分支中。

### 2.2.分支的合并

现在已经完成了`#16`的需求，需要将iss16分支合并到master分支。

```
$ git checkout master
Switched to branch 'master'
$ git merge iss16
Merge made by the 'recursive' strategy.
README.md | 1 +
1 file changed, 1 insertion(+)
```

此时分支的合并并不是`Fast-forward`的形式，master分支所在提交不是iss16所在提交的直接祖先，
在这种情况下，你的开发历史从一个更早的地方开始分叉开来。Git需要做一些额外的工作。


Git会使用两个分支的末端所指的快照(C4和C5)以及这两个分支的工作祖先(C2)，做一个简单的三方合并。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_35.png)
一次典型合并中所用到的三个快照

Git将三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它，这个被称作一次合并提交

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_24.png)
一个合并提交

Git会自行决定选取哪一个提交作为最优的共同祖先，并以此作为合并的基础。

当完成合并之后，需要删除iss16分支。

```
$ git checkout -d iss16
Deleted branch iss16
```

### 2.3.遇到冲突时的分支合并

如果在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git在合并时就会产生冲突：

```
$ git checkout master
Switched to branch 'master'
$ git merge iss16
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed;fix conflicts and then commit the result.
```

Git做了合并，但是没有自动地创建一个新的合并提交。Git会暂停下来，等待你去解决合并产生的冲突。

解决冲突的步骤：

* 第一步：使用`git status`命令查看包含合并冲突而处于未合并状态的文件。

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
Unmerged paths:
  (use "git add <file>..." to mark resolution)
  both modified: README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。

* 第二步：Git会在有冲突的文件中加入标准的冲突解决标记，你可以打开这些包含冲突的文件手动解决冲突。出现冲突的文件会包含一些特殊区段。

```
<<<<<<< HEAD:README.md
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss16:README.md
```

这表示HEAD所指示的版本(master分支所在的位置)在`=======`的上半部分，而`iss16`分支所指示的版本在`=======`的下半部分。

* 第三步：为了解决冲突，你必须选择使用由`=======`分割的两部分中的一个，或者也可以自行合并这些内容。

```
//选择上半部分或者下半部分
<div id="footer">
 please contact us at support@github.com
</div>
```

该解决方案仅保留了其中一个分支的修改，并且`<<<<<<<`,`=======`,和`>>>>>>>`这些行被完全删除。

当解决了所有文件里的冲突之后，对所有文件使用git add命令将其标记为冲突已解决。


自行合并冲突：`$ git mergetool`命令

该命令会为你启动一个合适的可视化合并工具，并带领你一步一步解决这些冲突.

```
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git meergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse
diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
Merging:
README.md

Normal merge conflict for 'README.md':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff):
```

当退出合并工具之后，Git会询问刚才的合并是否成功。如果回答是，Git会暂存那些文件以表明冲突已解决：

```
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
Changes to be committed:

  modified: README.md
```

* 第四步：当所有的有冲突的文件都已经暂存，就可以提交到Git仓库中。

```
$ git commit -m "solve merge branch"

Merge branch 'iss16'

Conflicts:
  README.md
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
# .git
# and try again.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#    modified: README.md
```

