---
title: 3-5-Git分支
comments: true
date: 2018-08-17 00:42:58
categories: 前端
tags: Git
about:

---

### 6.2.一个变基的例子

在对两个分支进行变基时，也可以指定另外的一个分支进行应用，就像从一个特性分支里再分出一个特性分支的提交历史。

###### 举例：

创建了一个特性分支server，为服务器添加了一些功能，提交了C3和C4，然后在C3上创建了特性分支client，为客户端添加了一些功能，提交了C8和C9，最后，回到了server分支，又提交了C10。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_39.png)
从一个特性分支里再分出一个特性分支的提交历史

#### 6.2.1.git rebase命名 + `--onto选项 `的使用

如果希望将client中的修改合并到主分支并发布，但是并不想合并server中的修改，也就是说，选中在client分支里但不在server分支里的修改(C8和C9)，将它们变基到master分支上。

```
$ git rebase --onto master server client
```

取出cilent分支，找出处于client分支和server分支的共同祖先之后的修改，然后把它们变基到master分支上。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_40.png)
截取特性分支上的另一个特性分支，然后变基到其他分支

这时，就可以切回到master分支，进行一次快进合并。

```
$ git checkout master
$ git merge client
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_41.png)
快速合并master分支，使之包含来自cilent分支的修改

#### 6.2.2.git rebase  [basebranch]  [topbranch]命令

git rebase  [basebranch]  [topbranch]命令可以直接将特性分支(server)变基到目标分支(master)上。

```
$ git rebase master server
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_42.png)
将server中的修改变基到master上

这时，就可以切回到master分支，进行一次快进合并。

```
$ git checkout master
$ git merge server
```

这时，client和server分支中的修改都已经整合到主分支中了，可以删除掉client和server分支了。

```
$ git branch -d client
$ git branch -d server
```

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_43.png)
最终的提交历史

### 6.3.变更的风险

#### 6.3.1.变基的准则---不要对在你的仓库外有副本的分支执行变基

变基操作实质是丢弃一些现有的提交，然后相应地新建一些内容但实际上不同的提交。如果已经将提交推送至某个仓库，其他人已经从该仓库拉取提交并进行了后续工作，如果再使用`git rebase`命令重新整理提交并再次推送，这时候就会变得一团糟。

###### 举例：(在公开的仓库上执行变基操作)

假设从一个中央服务器克隆并且在它的基础上进行一些开发，提交历史如下。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_44.png)
克隆一个仓库，然后在它的基础上进行了一些开发

然后，其他人向中央服务器提交了一些修改，其中包括一次合并，这时，又一次抓取了这些在远程分支上的修改，并将其合并到你本地的开发分支，然后你的提交历史就会变成这样：

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_45.png)
抓取别人的提交，合并到自己的开发分支

突然，这个人又决定把合并操作回滚，改用变基，使用`git push --force`命令更新了服务器上的提交历史。更新之后，如果再从服务器上抓取更新，就会发现会多出一些新的提交历史。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_46.png)
有人推送了经过变基的提交，并丢弃了你的本地开发所基于的一些提交

此时，如果执行`git pull`命令，将会合并来自两条提交历史的内容，生成一个新的和并提交。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_47.png)
将相同的内容又合并了一次，生成了一个新的提交

如果执行`git log`命令，会发现有两个提交的作者、日期、日志。并且是一样的，这会是非常混乱的。

#### 6.3.2.用变基解决变基

如果团队中的某人强制推送并覆盖了一些你所基于的提交，我们需要知道你做了哪些修改，以及他们覆盖了哪些提交。

###### 举例(有人推送了经过变基的提交，并丢弃了你的本地开发所基于的一些提交)

* 检查哪些提交是自己分支上独有的(C2，C3，C4，C6，C7)

* 检查其中哪些提交不是合并操作的结果(C2，C3，C4)

* 检查哪些提交在对方覆盖更新时并没有被纳入目标分支(只有C2和C3，C4就是C4´)

* 把查到的这些提交应用到my_ying/master上面

此时，将相同的内容又合并了一次，生成了一个新的提交中不同的结果，也就是说，在一个被变基然后强制推送的分支上再次执行变基。

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1268239/o_48.png)
在一个被变基然后强制推送的分支上再次执行变基

#### 6.3.3.变基VS合并

仓库的提交历史就是记录实际发生过什么，它是针对历史的文档，不能乱改，也就是说，只对尚未推送或分享给别人的本地修改指向变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样才是最好的选择。
