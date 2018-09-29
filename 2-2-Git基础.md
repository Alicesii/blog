---
title: 2-2-Git基础
comments: true
date: 2018-07-26 15:35:02
categories: 博客列表
tags: Git
about:

---

## 3.查看提交历史

`git log`命令用来查看某个项目的提交历史，我们可以查看提交了若干更新或者是克隆的某个项目提交历史记录。

```
$ git pull origin master
$ git log
commit 011909b7b4e8942a5f18a7bb52a25dd63623c3aa
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Sun Jul 29 18:03:16 2018 +0800

    blog-third

commit 2d428927038fc78846f7c4b21906bcede964226c
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Sun Jul 29 17:51:44 2018 +0800

    README.md

commit c96f1e2772d2c0a45734fa5f4ef68d1070812186
Author: ying <1511317497@qq.com>
Date:   Tue Jul 17 11:20:25 2018 +0800

    blog-second

commit d585eaa4e1eac4a181c55acf5ce045d76c901ceb
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Thu Jul 12 16:17:32 2018 +0800

    Create README.md
```

`git log`会按提交时间列出所有的更新，最近的更新排在最上面，这个命令会列出每个提交的`SHA-1`校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

git log命令有许多选项可以帮助你搜寻你所要找的提交。

* `-p`按补丁格式显示每次提交的内容差异，可以加上数字(-2)它将限制输出长度，也就意味着仅显示最近两次的提交。

```
$ git log -p -2
commit 011909b7b4e8942a5f18a7bb52a25dd63623c3aa
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Sun Jul 29 18:03:16 2018 +0800

    blog-third

diff --git "a/README.md" "b/README.md"
index aae07c6..4bfc990 100644
--- "a/README.md"
+++ "b/README.md"
@@ -165,7 +165,6 @@ A.prototype.name="张三";
 function B(){
 }
 B.prototype=new A();
-
 var b=new B();
 console.log(b.name);  //张三
 console.log(b.age);  //undefined
```

 -p选项除了显示基本信息之外，还附带了每次commit的变化。

* `--stat`显示每次更新的文件修改统计信息

```
$ git log --stat
commit 011909b7b4e8942a5f18a7bb52a25dd63623c3aa
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Sun Jul 29 18:03:16 2018 +0800

    blog-third

  README.md | 1 -
 1 file changed, 1 deletion(-)

commit 575b0cc7ab8a6ce8b107ac0e4b5bc7636b7897c1
Author: 255255255255 <33173766+255255255255@users.noreply.github.com>
Date:   Sun Jul 29 18:01:49 2018 +0800

    blog-third

 "README.md" | 2 --
 1 file changed, 2 deletions(-)
```

--stat选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。

* `--shortstat`：只显示`--stat`中最后的行数修改添加移除统计。

* `--name-only`：仅在提交信息后显示已修改的文件清单。

* `--name-status`：显示新增、修改、删除的文件清单。

* `--abbrev-commit`：仅显示SHA-1的前几个字符，而非所有的40个字符。

* `--relative-date`：使用较短的相对时间显示

* `--graph`：显示ASCCII图形表示的分支合并历史。

* `--pretty`：指定使用不同于默认格式的方式展示提交历史。可用的选项包括oneline，short，full，fuller 和format。

oneline表示将每个提交放在一行显示。

```
$ git log --pretty=oneline
011909b7b4e8942a5f18a7bb52a25dd63623c3aa blog-third
2d428927038fc78846f7c4b21906bcede964226c README.md
5f007ee5cf5fa337420265c713b5717c72763500 first-javascript
c96f1e2772d2c0a45734fa5f4ef68d1070812186 blog-second
d585eaa4e1eac4a181c55acf5ce045d76c901ceb Create README.md
dceb7854ab9a316e3800046852f399dde075aaed Delete README.md
abd37e003368157d02f6f9e2e948ff72322c9890 blog-first
```

format可以定制要显示的记录格式。

%H 提交对象（commit）的完整哈希字串

%h 提交对象的简短哈希字串

%T 树对象（tree）的完整哈希字串

%t 树对象的简短哈希字串

%P 父对象（parent）的完整哈希字串

%p 父对象的简短哈希字串

%an 作者（author）的名字

%ae 作者的电子邮件地址

%ad 作者修订日期（可以用--date=选项定制格式）

%ar 作者修订日期，按多久以前的方式显示

%cn 提交者（committer）的名字

%ce 提交者的电子邮件地址

%cd 提交日期

%cr 提交日期，按多久以前的方式显示

%s 提交说明

```
$ git log --pretty=format:"%h - %an,%ar : %s"
011909b - 255255255255,3 hours ago : blog-third
2d42892 - 255255255255,3 hours ago : README.md
5f007ee - ying,7 days ago : first-javascript
c96f1e2 - ying,12 days ago : blog-second
d585eaa - 255255255255,2 weeks ago : Create README.md
dceb785 - 255255255255,2 weeks ago : Delete README.md
abd37e0 - ying,2 weeks ago : blog-first
```

注意：作者和提交者的差别

作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。假设当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是作者，而那个核心成员就是提交者。

我们可以灵活的组合使用git log命令的这些选项。

```
$  git log --pretty=format:"%h %s" --graph
* 011909b blog-third
* 575b0cc blog-third
* 9dcb0d7 blog-third
* 050b346 blog-third
* 2d42892 README.md
* 9e2b474 README.md
* 5f007ee first-javascript
* c96f1e2 blog-second
* d585eaa Create README.md
* dceb785 Delete README.md
* abd37e0 blog-first
```

### 3.1.限制输出长度

我们不仅可以定制输出格式的选项，还可以限制输出长度，也就是只输出部分提交信息。

* -(n)仅显示最近的n条提交

```
$ git log -p -2
```

n可以是任何整数，表示仅显示最近的若干条提交

* --since, --after仅显示指定时间之后的提交。

```
$ git log --since=2.weeks
$ git log --since="2018-07-29"
$ git log --since= "2 years 1 day 3 minutes ago"
```

* --until, --before仅显示指定时间之前的提交。

* --author 仅显示指定作者相关的提交。

* --committer 仅显示指定提交者相关的提交。

* --grep仅显示含指定关键字的提交

* -S仅显示添加或移除了某个关键字的提交。例如，想找出添加或移除了某一个特定函数的引用的提交。

```
$ git log -Sfunction_name
```

举例：要查看Git仓库中，2018年7月期间，作者ying提交的文件。

```
$ git log --pretty=format:"%h - %s" --author=ying --since="2018-07-01" --before="2018-08-01"

7022a10 - second-Git
9358d27 - first-Git
```

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
