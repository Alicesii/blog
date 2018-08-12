---
title: Gulp详解
comments: true
date: 2018-07-02 09:56:31
categories: 博客列表
tags: Gulp
about:

---

> 一名合格的前端开发人员，怎么可能不想办法尽可能的做到更好的性能优化。如果可以合理的利用一些构建工具，例如`Gulp`、`Grunt`等，这样可以达到事半功倍的效果，本篇文章就来介绍一下`Gulp`的基本用法。

## Gulp是什么以及用来做什么？

gulp是基于node的一个自动化构建工具

用途：

* 1、搭建Web服务器

* 2、浏览器自动刷新

* 3、使用预处理器Sass、Less

* 4、浏览器的兼容处理(添加前缀)

* 5、外部文件的自动压缩(图片文件、css文件、JS文件)

## 创建Gulp工具

我们需要下载Gulp,gulp是基于node的一个自动化构建工具,node可以做网站、但是更大的用处是用作构建工具的。

（1）package.json文件

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_8.png)

（2）下载gulp：

* 下载全局的gulp

路径地址：C:\Users\dell\AppData\Roaming\npm\node_modules\gulp\bin\gulp.js
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_9.png)

* 下载非全局gulp
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_10.png)

我们首先的搭建一个像样的工程的目录结构：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_1.png)

`gulpfile.js`就是我们需要编写的地方了,加油吧！！！

## 第一步：引入gulp

`var gulp = require('gulp');`

这行命令告知Node去`node_modules`中查找`gulp`包，先局部查找，找不到就去全局环境中查找。找到之后就会赋值给`gulp`变量，然后我们就可以使用它了。

## 第二步：创建一个简单任务：

```
gulp.task('task-name', function() {

});
```

`task-name`是任务的名字，我们想要查看运行结果的话，就在Git中输入`gulp task-name`，将运行该任务。

我们可以测试一下：(在git中输入`gulp hello`)

```javascript
var gulp = require('gulp');
gulp.task('hello', function() {
  console.log('Hello World!');
});
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_2.png)


我们可以看到成功输出了`Hello World`，但是如果你真的认为Gulp这么简单的话，那你就错了,我们来看一些我们在平时的项目开发中可以用到的一些。


## 第三步：创建复杂任务

`Gulp`任务通常会包含两个特定的`Gulp`方法和一些`Gulp`插件。

```javascript
gulp.task('task-name', function () {
  return gulp.src('source-files') // 输入源文件的地址
    .pipe(aGulpPlugin()) // 通过一个Gulp插件发送
    .pipe(gulp.dest('destination')) // 输出目标文件的地址
 })
```

解释：

* `gulp().task`:配置我们的具体任务

* `gulp.src()`：获取需要构建资源的路径，传递的参数必须是一个路径。

* `gulp.dest()`：放置我们构建好的资源的路径，传递的参数也必须是一个路径。

* `pipe()`：类似于管道的作用，起到一个'承上启下'的作用，上一次的处理结果，当做下一次传递参数。

注意：Gulp的开发者很厉害，所有的功能都是采用插件的机制，你需要用什么功能你直接安装插件、引入就可以使用了，这样做避免了将所有的方法都封装在Gulp当中，避免了Gulp过于庞大、笨重的问题。Gulp只是有个这样的任务`gulp().task`，但是具体的功能还是要依靠安装插件来实现。说的通俗一点就是Gulp什么事情都不干，专门`"指挥"`别人干。

## 第四步：将`.sass`文件转化成`.css`文件。

需要给`sass`任务提供源文件和输出位置,所以我们先在项目中创建`app/scss`文件夹,里面有个`styles.scss`文件。

在第三个步骤中我们说过Gulp任务通常会包含两个特定的`Gulp`方法和一些`Gulp`插件，现在我们需要先安装`gulp-sass`插件我们再来写具体的方法。

* 插件安装：`npm install gulp-sass --save-dev`


* 引入gulp和sass。

```
var gulp=require("gulp");
var sass=require("gulp-sass");
```

* 输入和输出的方法

`sass`处理之后，我们希望它生成`css`文件并产出到`app/css`目录下`(style.css)`，可以这样写

```
var gulp=require("gulp");
var sass=require("gulp-sass");
gulp.task('sass',function(){
	return gulp.scr("app/scss/styles.scss")//源文件位置
         .pipe(sass())
         .pipe(gulp.dest("app/css")) //目标位置。
})
```

好了，我们先在需要测试一下，在`style.scss`中写入一些`scss`命令，

```javascript
//自定义变量
$color:pink;
.test1{
    background-color:$color;
}
```

使用`Git`执行`gulp sass`,你将看到`app/css/`目录中多了`styles.css`文件,并且有下面的代码.

```javascript
.test1 {
  background-color: pink;
 }
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_3.png)

这样我们就把`.sass`文件转化成`.css`文件，这个是Gulp自动转化成的，我们就只需要写几行代码就可以办到了，我们使用sass写css样式大大简化了操作，提高了开发效率。在实际的开发过程中。不只有一个.sass文件，我们可以使用Node通配符来匹配多个`.css`文件,我们应该学会合理的使用正则表达式。

## 第五步：Node通配符了解一下

使用通配符，计算机检查文件名和路径进行匹配。

大部分时候，我们只需要用到下面4种匹配模式：

* `*.scss：`:`*`号匹配当前目录任意文件，所以这里`*.scss`匹配当前目录下所有scss文件

* `**/*.scss：`:匹配当前目录及其子目录下的所有scss文件。

* `!not-me.scss：`:`！`号移除匹配的文件，这里将移除`not-me.scss`

* `*.+(scss|sass)`：`+`号后面会跟着圆括号，里面的元素用`|`分割，匹配多个选项。这里将匹配scss和sass文件。


现在需要将任何app下的scss文件，在执行命令之后将生成对应的css文件存放到`app/css/`。


```
var gulp=require('gulp');
var sass=require('gulp-sass');
gulp.task('sass',function(){
	return gulp.src("app/sass/**/*.scss")
	        .pipe(sass())
	        .pipe(gulp.dest("app/css"))
	})
```


## 第六步：给所有的css添加前缀

给css添加不同的浏览器内核的前缀有着重要的意义，这个可以很好地解决浏览器之间的兼容性问题，如果让我们一个一个去加的话那也太麻烦了，幸好Gulp可以帮助我们自动完成这个繁琐的任务，那就需要用到`autoprefixer`插件了。

* 插件的安装：`npm install  gulp-autoprefixer --save-dev`

* 引入`var autoprefixer=require("gulp-autoprefixer")`;

* 输入输出函数：

```
var autoprefixer=require("gulp-autoprefixer");
gulp.task('css',function(){
  return gulp.src("app/css/**/*.css")
  //pipe()方法：前一次的结果会当成后一次的输入
             .pipe(autoprefixer())
             .pipe(gulp.dest("dist"));
})
```

## 第六步：开启自动监听

我们有没有发现每次我们写一些新的功能的时候，我们都要输入`gulp task-name`，这样是不是超级麻烦，我们需要自动监听。

语法格式如下：

```
gulp.watch('files-to-watch', ['tasks', 'to', 'run']);

```


* 监听一个文件：`gulp.watch('app/scss/**/*.scss', ['sass'])`;

* 监听一个任务

```
gulp.task('watch', function(){
  gulp.watch('app/scss/**/*.scss', ['sass']);
})

```

我们可以来测试一下：

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');
gulp.task('sass', function() {
	return gulp.src('app/scss/styles.scss')
		.pipe(sass())
		.pipe(gulp.dest('app/css'))
});
gulp.task('watch', function(){
gulp.watch('app/scss/**/*.scss', ['sass']);
})
```


在Gulp中执行`gulp watch`,可以看到现在的从`.sass`文件到`.css`文件的转换过程是处于监听状态的，

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_4.png)

也就是说，我们每次修改`.sass`文件，就有Gulp帮我们自动执行，不用再手动执行了。这是一张盗的图,很炫酷的图。

![ ](https://dn-w3ctrain.qbox.me/gulp-for-beginner-watch-compile.gif)


## 第七步：浏览器的自动刷新(Browser Sync)

Gulp可不可以帮忙自动刷新浏览器呢，好期待呀,可以试一下。

* 插件的安装：`npm install browser-sync --save-dev`

* 引入sync

```
var gulp=require("gulp");
var sass=require("gulp-sass");
var sync=require("gulp-sync");
```

* 输入和输出的方法

创建一个`broswerSync`任务，我们需要告知它，根目录在哪里

```
gulp.task('sync',function(){
	browserSync({
    server: {
      baseDir: 'app'  //根目录的位置
    }
  })
})
```

* 修改一下任务sass，让每次css文件更改都刷新一下浏览器

```javascript
gulp.task('sass', function() {
	return gulp.src('app/scss/styles.scss')
		.pipe(sass())
		.pipe(gulp.dest('app/css'))
		.pipe(browserSync.reload({
         stream: true
     }))
});
```

注意：我们必须在watch任务之前告知Gulp，先把任务`browserSync`和任务`Sass`执行了再说。

语法格式如下:

```
gulp.task('watch', ['array', 'of', 'tasks', 'to', 'complete','before', 'watch'], function (){

})
```

终极板的浏览器自动刷新效果:

```javascript
var gulp = require('gulp');
// Requires the gulp-sass plugin
var sass = require('gulp-sass');
var browserSync = require('browser-sync');
gulp.task('browserSync', function() {
  browserSync({
    server: {
      baseDir: 'app'//根目录的位置
    },
  })
})
gulp.task('sass', function() {
  return gulp.src('app/scss/**/*.scss')
    .pipe(sass())
    .pipe(gulp.dest('app/css'))
    .pipe(browserSync.reload({
      stream: true
    }))
});   //在监听之前先执行browserSync和sass函数
gulp.task('watch', ['browserSync', 'sass'], function (){
  gulp.watch('app/scss/**/*.scss', ['sass']);
})
```

当执行`gulp watch`命令之后，首先会执行`browserSync`在执行`Sass`，才会开始监听。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_5.png)

并且现在浏览器的显示的页面为`app/index.html`。修改了`styles.scss`之后，浏览器会自动刷新。

![ ](https://dn-w3ctrain.qbox.me/gulp-for-beginner-bs-change-bg.gif)

我们已经让`.sass`文件自动刷新了，应该让所有的文件都实现自动刷新的效果。

```
gulp.task('watch', ['browserSync', 'sass'], function (){
  gulp.watch('app/scss/**/*.scss', ['sass']);
  gulp.watch('app/*.html', browserSync.reload);
  gulp.watch('app/js/**/*.js', browserSync.reload);
});
```

## 第八步：合并多个JS文件。

一般我们引入的JS文件都是这样的，路径都不太相同，那么该怎么合并呢?

```
<script type="text/javascript" src="js/lib/commonobj.js" ></script>
<script type="text/javascript" src="js/lib/require.js" ></script>
```

`gulp-useref`插件会将多个文件拼接成单一文件，并输出到相应目录。我们需要把合并的信息写到`<!--bulid-->`当中。

```javascript
<!-- build:<type> <path> -->
    在这里引入外部资源文件
<!-- endbuild -->
```

不仅需要明确要压缩的文件的路径，还必须知道输出的路径，最终的产出为main.min.js

```javascript

<!--build:js js/main.min.js -->
		<script type="text/javascript" src="js/lib/commonobj.js" ></script>
		<script type="text/javascript" src="js/lib/require.js" ></script>
<!-- endbuild -->
```

* 插件安装：`npm install gulp-useref --save-dev`

* 引用：`var useref = require('gulp-useref');`

* 输入和输出的方法：

```javascript
gulp.task('useref', function(){
  return gulp.src('app/*.html')
        .pipe(useref())
        .pipe(gulp.dest('dist'));
});
```

执行 `gulp useref`命令，Gulp将合并三个script标签成一个文件，并保存到`dist/js/main.min.js`。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_6.png)

## 第九步：压缩合并后的JS文件

* 插件安装：`npm install gulp-uglify --save-dev`

* 引用：`var uglify = require('gulp-uglify');`

```
gulp.task('useref', function(){
  return gulp.src('app/*.html')
    .pipe(uglify())
    .pipe(useref())
    .pipe(gulp.dest('dist'))
});
```

执行 `gulp useref`命令，`dist/js/main.min.js`中的文件就被压缩了。

这下压缩、合并就搞定了，开心开心......

现在执行完`gulp useref`命令后，我们会发现`.html`文件中的script路径将只剩下`main.min.js`，这是很正常的事情，因为已经生成新的`main.min.js`文件，并且体积更小，肯定引入的是最新生成的JS文件。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1246145/o_7.png)

## 第十步：压缩CSS文件

* 插件安装：`npm install gulp-if gulp-minify-css --save-dev`

* 引用：

```
var gulpIf = require('gulp-if');
var minifyCSS = require('gulp-minify-css');
```

* 输入输出方法：

```javascript

gulp.task('useref', function(){
  return gulp.src('app/*.html')
    // 只有当它是一个CSS文件时才会出现
    .pipe(gulpIf('*.css', minifyCSS()))
    // 只有当它是一个Javascript文件时才会出现
    .pipe(gulpIf('*.js', uglify()))
    .pipe(useref())
    .pipe(gulp.dest('dist'))
});
```

## 第十一步：图片文件处理

* 插件安装：`npm install gulp-imagemin --save-dev`

* 引入：`var imagemin = require('gulp-imagemin');`

* 输入输出方法：

```
gulp.task('images', function(){
  return gulp.src('app/images/**/*.+(png|jpg|gif|svg)')
  .pipe(imagemin())
  .pipe(gulp.dest('dist/images'))
});
```

压缩图片可能会占用较长时间，使用 `gulp-cache `插件可以减少重复压缩。

* 插件安装：`npm install gulp-cache --save-dev`

* 引入：`var cache = require('gulp-cache');`

* 输入输出：

```
gulp.task('images', function(){
  return gulp.src('app/images/**/*.+(png|jpg|jpeg|gif|svg)')
  .pipe(cache(imagemin({
      interlaced: true
    })))
  .pipe(gulp.dest('dist/images'))
});
```

## 第十二步：清理生成文件

自动生成了新的压缩版文件之后，就可以把旧的未压缩的、分散的文件给删除了。

* 插件安装：`npm install del --save-dev`

* 引入`var del = require('del');`

* 输入输出函数：

```
gulp.task('clean', function() {
  del('dist');
});
```

但是我们又不想图片被删除，因为图片改动的几率不大,这里就得利用正则表达式了。

```
gulp.task('clean:dist', function(callback){
  del(['dist/**/*', '!dist/images', '!dist/images/**/*'], callback)
});

```

这个任务会删除，除了`images/`文件夹，dist下的任意文件。

为了知道`clean:dist`任务什么时候完成，我们需要提供callback参数。

在某些时候我们还是需要清除图片，所以clean任务我们还需要保留。

```
gulp.task('clean', function(callback) {
  del('dist');
  return cache.clearAll(callback);
})
```

在实际的项目中可能用到的就只有这些了，我们需要组合一下这些`task`,组合成整体。

## 第十三步：整合

通篇文章总共有两方面的内容：

* 第一是开发过程，Sass的使用，监听文件，刷新浏览器。

* 第二是优化，优化CSS,JavaScript,压缩图片，并把资源从app移动到dist

第一个方面：

```
gulp.task('watch', ['browserSync', 'sass'], function (){
})

```

第二个方面：

```
gulp.task('build', [`clean`, `sass`, `useref`, `images`, `fonts`], function(){

})
```

对于第二个来说，这样`Gulp`会同时触发多个事件，这样执行的逻辑是不对的，我们要让`clean`在其他任务之前完成。

这里需要用到`RunSequence`插件

* 插件安装：`$ npm install run-sequence --save-dev`

* 引入：`var runSequence = require('run-sequence')`;

语法格式：

```
gulp.task('task-name', function(callback) {
  runSequence('task-one', 'task-two', 'task-three', callback);
});
```

执行`task-name`时，Gulp会按照顺序执行`task-one,task-two,task-thre`。

`RunSequence`也允许你同时执行多个任务。

```
gulp.task('task-name', function(callback) {
  runSequence('task-one', ['tasks','two','run','in','parallel'], 'task-three', callback);
});
```

我们需要改变一下代码

* 第二条线路：

```
gulp.task('build', function (callback) {
  runSequence('clean:dist',
    ['sass', 'useref', 'images', 'fonts'],
    callback
  )
})
```

第一条线路：

```
gulp.task('gulpdemo', function (callback) {
  runSequence(['sass','browserSync', 'watch'],
    callback
  )
})
```

现在只需要执行 `gulp gulpdemo`命令,就可以完成上述所有的功能。

参考链接：https://w3ctrain.com/2015/12/22/gulp-for-beginners/?utm_source=tuicool&utm_medium=referral

最终版的代码在我的Github仓库，大家可以看一下。
