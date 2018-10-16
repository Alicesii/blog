---
title: Hexo+Github搭建个人博客
comments: true
date: 2018-04-01 16:35:14
categories: 前端
tags: Hexo Github
img:

---
>这是我的第一篇博客，我就说说我是怎么利用Hexo和GitHub搭建博客的吧，可以在外网上访问我的博客，心情还是很激动的。

### 1.所需的软件

* 1.nodeJs
* 2.Git(Git Bash)
* 3.GitHub账号

### 2.Hexo官网的操作

* 2.1.进入[Hexo](https://hexo.io)官网，进入之后往最下面拉，总有五条命令,这个时候Git Bash就派上用场了。

```javascript
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server

```

* 2.2.新建一个文件夹Hexo，点击右键，就可以看到Git Bash的快捷键方式，

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_3.png)
单击之后就进入到了可以安装Hexo环境了，这五条指令一条一条的依次输入执行，不能一次性粘贴复制，否则会报错。

* 2.3.输入`hexo server`后就可以在浏览器输入`localhost:4000`查看,如果出现了一个简易的博客页面而不是一个404页面，恭喜你这是成功的第一步。

### 3.GitHub官网的操作

* 3.1.[GitHub官网](https://github.com)，进入之后，首先得登录就不用说了，进去之后你会发现右上角会有一个加号➕，鼠标放上去会出现四个选项，首先选择第四个[new organization]也就是新建一个组织，随便起一个名字就可以了，然后选择第一个[new repository]，也就是新建一个仓库，填入仓库的名字。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_5.png)

**注意：仓库的名字和组织的名字一定要相同，一定要是name.github.io格式，不然会出错，大家一定要记住。**

* 3.2.生成SSH密钥

在这里又要用到GitBush了，`ssh-keygen -t rsa`输入该命令之后，一路回车，就会生成密钥了。是不是特别想知道生成的密钥的位置，就在**C\用户\你的电脑\.ssh文件夹**里面，其中有两个密钥，用记事本打开其中的`id_rsa.pub`复制即可,然后回到GitHub中，点击右上角图片的位置，会出现settings(设置)点击进去，在鼠标下拉的过程中你会发现`SSH and GPG keys`这个按钮，不要犹豫，赶紧点进去，然后点击`new ssh key`，将你刚才复制的密钥粘贴进去，点击保存。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_4.png)

* 3.3.配置文件

配置文件的路径是`F:\Hexo\blog`中的`_config.yml`，可以参考[官网的配置文件](https://hexo.io/zh-cn/docs/configuration.html)，官网的配置文件特别详细，如果是第一次修改的，就改改网站(site)里面的标题、副标题、网站的名字其他的不要乱改，最重要的配置是文件的最后一行`deploy`的配置如下图所示。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_2.png)

注意： repository中的地址Github的远程仓库的地址，每个人的肯定都不一样，大家根据情况自行修改。配置文件时，大家可以一步一步来配置，不断地利用`hexo s`，然后刷新localhost:4000页面检查自己是否配置的正确

### 4.上传文件

* 4.1.上传文件的操作
上传文件时又要用到`Git Bash`了，大家应该知道Git的重要性以及方便性，上传到GitHub上面的命令是：

```
$ hexo clean
$ hexo g
$ hexo d
```
 

注意：在Git Bash里面输入这三条代码之前，你首先必须确认你有没有安装node，检测node有没有安装的指定是，如果没有安装node,请输入指令`npm install node -g`全局安装
 
* 4.2.GitHub Pages的设置

这时候，你可以看到在你的Github中看到你所传上去的代码了，这时候你还需要点击设置`Settings`按钮，找到`GitHub Pages`设置你的source为master分支，点击Save存储按钮，如果你可以看到`Your site is published at https://yingy0.github.io/`这段提示信息，说明你可以在外网上访问你的博客了，[我的博客的地址https://yingy0.github.io/](https://yingy0.github.io/)， 因为每个人的name都不一样，大家就只需要改下名字就好。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1239045/o_1.png)


### 5.上传博客

* 5.1.新建md文件

在Github中的默认文件就是md文件，至于md文件的优点我也就不啰嗦了，新建md文件的指令是`hexo new "postName"`其中postName为文件标题名称,新建的md文件默认有标题信息和创建的日期信息等，新建文件的默认路径在 `F:\Hexo\blog\source\_posts`

* 5.2.md文件的编写

md文件的写法是有一定要求的，大家就自行百度，编写完成`hexo s`之后就可以在本地浏览器看一下效果`localhost:4000`，确定没有问题之后上传到GitHub上面。

```javascript
$ hexo clean
$ hexo g
$ hexo d
```
### 6.换主题

* 6.1.下载主题

当你可以在外网上面访问你的主题时，你是不是会觉得你的网站有些许难看，不像别人的那么炫酷，当然你可做到，首先在[官网](https://hexo.io/themes/)上面下载一个你喜欢的主题，下载好的主题放在`F:\Hexo\blog\themes`，现在就开始来配置吧，主题的配置文件的路径不同，不要和上面提到的配置文件搞混了.

* 6.2.主题的配置文件

打开主题的配置页，路径是`F:\Hexo\blog\themes\cyanstyle`我下载的主题是cyanstyle，每个人的喜好不同，大家自行参考，该主题的menu中默认有Home和Archives，我在我的menu菜单中添加了About。

```javascript
menu:
  Home: /
  Archives: /archives
  About: /about
```

在不同的主题中还可以添加`categories`等，配置文件中只需要该个menu，其他的也不需要在动了。
 
* 6.3.设置Tags标签页

输入`hexo new page tags`该命令之后，在`F:\Hexo\blog\themes\cyanstyle\source`下会新生成一个新的文件夹tags，在该文件夹下会有一个index.md文件。

* 6.4.设置About

输入`$ hexo new page about`该命令之后，在`F:\Hexo\blog\themes\cyanstyle\source`下会新生成一个新的文件夹about,在该文件夹下会有一个index.md文件。
 现在当你新建一个md文件时`hexo new "postName"`，就会出现tags、title、日期等，当你填上不同的标题、不同的标签页之后，浏览器会自动的为你归档。

```javascript
title: Hexo+Github搭建个人博客
date: 2018-04-22 18:14:02
tags: Hexo Github
```
在编写主题的配置文件时，一定要参考你所下载的官方主题的说明文档，不能只看着参考教程写，因为你参考的教程可能与你下载的主题不是一一对应的，这样可能会产生冲突，带来很严重的后果。


