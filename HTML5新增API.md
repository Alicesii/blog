---
title: HTML5新增APT
comments: true
date: 2018-06-27 10:47:01
categories: 博客列表
tags: HTML5
about:

---

## 优点：


* 极简的DOM操作

* 本地数据缓存：不管是取数据，还是存数据、还是删除数据也好都十分的简单，只需要调用相对应的方法即可。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_1.png)


* 提升了浏览器的存储性能

通常来讲，再更老版本的浏览器，如果用户想要给本地写数据是非常困难的，可以通过cookie来操作，相对比localStorage来说，cookie的用法看起来就太繁琐了。

* 跟硬件相关的特性

可以通过Google的服务获取我们当前设备所在的地理位置

可以获得从打开页面开始那一刻页面性能的监控：`window.performance.timing`,后面我们在具体讲解。

* HTML5 API更好的完成性能上的提升

`Web worker`异步请求、离线缓存、应用程序缓存为应用带来三个优势：

**离线浏览 - 用户可在应用离线时使用它们**

**速度 - 已缓存资源加载得更快**

**减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源。**

## 1.DOM操作

使用原生JavaScript获得DOM节点对象的时候有以下几种方式：

```JavaScript
document.getElementById("#elementId");

document.getElementsByName("elementName");

document.getElementsByTagName("tagName");
```

我们虽然可以使用上面的几种方式方便获得元素，但也存在着很大的缺点，比如根据Class来获取元素，如果有需要，我们要么自己写`getElementsByClassName()`方法，要么使用其他的JS类库，使用类库也存在着问题：一是加载类库会影响网站性能，二是还必须学习类库的相关知识。现在HTML5给我们提供了一些API，可以很方便的解决这个问题。


### 1.1.`document.querySelector("selector")`

* 根据css选择器返回第一个匹配的元素，如果没有匹配返回null；

* 浏览器支持: Chrome 4.0+, FireFox 3.5+, Safari 3.2+, Opera 10.1+, IE 8+

举例：

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<ul>
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
		<script>
			var selector = document.querySelector('li');
			console.log(selector.innerHTML);
		</script>
	</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_4.png)


* `querySelector`也支持多个选择器

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<ul class="header">
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
		<script>
		 var selector = document.querySelector('.header li');
		 console.log(selector.innerHTML);
		</script>
	</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_4.png)

可以看到`querySelector`非常的方便，类似jquery的选择器，也不存在兼容性问题。

### 1.2.`document.querySelectorAll("selector")`;

* `querySelectorAll`和`querySelector`作用一样的，只是`querySelectorAll`返回的是元素数组，`querySelector`返回的是一个元素。如果`querySelectorAll`没有匹配的内容返回的是一个空数组。

* 浏览器支持： Chrome 4.0+, FireFox 3.5+, Safari 3.2+, Opera 10.1+, IE 8+


```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<ul class="header">
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
	  <script>
    var selector = document.querySelectorAll('.header li');
	console.log(console.log(selector););
		</script>
	</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_5.png)

### 1.3.`document.getElementsByClassName("selector")`


* `getElementsByClassName`是一个类选择器，返回值是一个元素数组。

* 浏览器支持： Chrome 4.0+, FireFox 3.5+, Safari 3.2+, Opera 10.1+, IE 8+

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<ul class="header">
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
		<script>
	  var selector = document.getElementsByClassName('header');
	 console.log(selector[0].innerHTML);
		</script>
	</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_6.png)

## 2.`classList`属性

classList属性没有出现之前JavaScript操作class类选择器使用的是`className()`，可以看下DOM对象的添加样式、删除样式、切换样式。

```JavaScript
//javascript只能用className()这样一个方法来判断存不存在样式、
添加样式、删除样式、切换样式。

```javascript

window.onload=function(){
//判断有没有样式
function hasClass(obj,cls){
    return obj.className.match(new RegExp('(\\s|^)'+cls+'(\\s|$)'));
}

//添加样式
function addClass(obj,cls){
    if(!hasClass(obj,cls)){
        obj.className+=" "+cls
    }
}

//删除样式
function removeClass(obj,cls){
    if(hasClass(obj,cls)){
        var reg=new RegExp('(\\s|^)'+cls+'(\\s|$)');
        obj.className=obj.className.replace(reg,' ');
    }
}

//切换样式
function toggleClass(obj,cls){
    if(hasClass(obj,cls)){
        removeClass(obj,cls);
    }else{
        addClass(obj,cls);
    }
  }
}

```

### 2.1.`classList`属性

`classList`是HTML5新的属性，只不过现在浏览器支持的不是很好,我们使用[caniuse](https://caniuse.com/)网站检测一下：

中文：
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_7.png)

英文：
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_7.1.png)


判断是否是IE11+以及其他现代浏览器:

`document.body.classList`是否为`undefined`

我们把上面的实例修改为classlist来操作：

```JavaScript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <ul class="class1 class2 class3 ">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
    <script>
    var ul = document.getElementsByTagName("ul")[0];
    console.log(ul.classList);
    </script>
</body>
</html>
```

下面是Chorme浏览器下面打印的结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_8.png)

可见其直接暴露的API有：

* `length`属性，表示元素类名的个数，只读

* `item()`支持一个参数，为类名的索引，返回对应的类名，例如上例：

```JavaScript
ul.classList.item(0);

//结果是："class1".
```

如果索引超出范围，例如：

```
ul.classList.item(3);

结果是：null.
```

* `add()`：支持一个类名字符串参数。表示往类名列表中新增一个类名；如果之前类名存在，则添加忽略。例如：

```JavaScript
ul.classList.add("class1");

ul.classList.length ;   // 3
```

`add()`方法执行的返回值是`undefined`

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_9.png)

 因此，classList的add()方法是无法级联的，remove()方法也是无法级联的。

* `remove()`：支持一个类名字符串参数。表示往类名列表中移除该类名。例如：

```JavaScript
ul.classList.remove("class1");

ul.classList.length    // 2
```


类似于jQuery对象中的`removeClass()`方法，然后者返回包装器对象本身，`removeClass()`方法可级联；

`remove()`方法返回undefined，索引`remove()`方法是无法级联的。

* `toggle()`：支持一个类名字符串参数。无则加勉，有则移除之意。若类名列表中有此类名，移除之，并返回false; 如果没有，则添加该类名，并返回true.

* `contains()`：支持一个类名字符串参数。表示往类名列表中是否包含该类名。有点对应jQuery中的hasClass方法。如果包含，则返回true, 不包含，则false。

举例：

`ul.classList.contains("class1");  // false 因为"class1"上面remove掉了`


## 3.全屏

为了方便用户的阅读或者观看视频，很多的网站实现了全屏功能。`FullScreen API` 是一个新的JavaScript API,简单而又强大.`FullScreen` 让我们可以通过编程的方式来向用户请求全屏显示,如果交互完成,随时可以退出全屏状态.

FullScreen是HTML5的一个新特征，现在主流的浏览器已经支持,我们可以使用[caniuse](https://caniuse.com/)网站再检测一下：

中文：
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_10.png)

英文：
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_10.1.png)

### 3.1.`FullScreen API `接口

属性：

* `fullscreenElement`：该属性返回当前处于全屏模式的DOM元素。

* `fullscreenEnabled`：该属性返回当前`document`是否进入了可以请求全屏模式的状态。


方法：

* `requestFullscreen() `：请求进入全屏模式。

* `exitFullscreen() `：退出全屏模式。

事件：

* `fullscreenchange `：进入/退出全屏模式切换时会触发。

* `fullscreenerror `：进入/退出全屏模式失败时会触发。

由于各个浏览器对`FullScreen`接口实现的方式不一样，主要是因为浏览器内核的差别，所以在使用的时候要考虑浏览器的兼容性。


### 3.1.进入全屏

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		    <button id="btn" onclick="launchFullScreen()">我是全屏</button>
		<script>
			// 找到支持的方法, 使用需要全屏的 element 调用
			function launchFullScreen(element) {
				var element = element || document.documentElement;
				if(element.requestFullscreen) {
					element.requestFullscreen();
				} else if(element.mozRequestFullScreen) {
					element.mozRequestFullScreen();
				} else if(element.webkitRequestFullscreen) {
					element.webkitRequestFullscreen();
				} else if(element.msRequestFullscreen) {
					element.msRequestFullscreen();
				}
			}
		</script>
	</body>
</html>
```

有的浏览器在进入全屏的时候会提示用户是否进行全屏，如果用户取消则全屏失效。同意浏览器的工具栏以及浏览器其它的组件都会隐藏起来，使document的高和宽横跨整个屏幕。

一般浏览器在进入全屏的时候提供用户按esc键可以退出全屏，即使这样有的时候我们还是需要给用户提供退出全屏的操作。

### 3.2.退出全屏

```JavaScript
 <script>
	// 退出 fullscreen
	function exitFullscreen() {
		if(document.exitFullscreen) {
			document.exitFullscreen();
		} else if(document.mozExitFullScreen) {
			document.mozExitFullScreen();
		} else if(document.webkitExitFullscreen) {
			document.webkitExitFullscreen();
		}
	}
</script>

```

退出全屏的代码很简单，我们只需要考虑浏览器的前缀就可以了。

注意: `exitFullscreen`只能通过`document`对象调用,而不是使用普通的`DOM element`.

### 3.3.检查全屏状态变化

有的时候为了用户友好体验，在进入全屏或者退出全屏的时候，需要给用户提示，这个时候我们可以使用`FullScreen`的`screenchang`事件进行监控。

```JavaScript
<script>
	document.addEventListener("fullscreenchange", function() {
		fullscreenState.innerHTML = (document.fullscreen) ? "" : "not ";
	}, false);
	document.addEventListener("mozfullscreenchange", function() {
		fullscreenState.innerHTML = (document.mozFullScreen) ? "" : "not ";
	}, false);
	document.addEventListener("webkitfullscreenchange", function() {
		fullscreenState.innerHTML = (document.webkitIsFullScreen) ? "" : "not ";
	}, false);
</script>
```

### 3.4.css的全屏样式`Styling fullscreen`

在css中，我们有几个伪类来给全屏设置样式,一般是`full-screen`这个伪类，然后会自动再全屏的时候生效

```JavaScript
<style>
	html:-moz-full-screen {
		background: red;
	}
	
	html:-webkit-full-screen {
		background: red;
	}
	
	html:fullscreen {
		background: red;
	}
</style>
```

全屏状态下的键盘输入 `Full screen with key input`

 为了安全原因，很多情况下全屏输入都是被阻塞禁止的，但是chrome允许通过下面的API来允许键盘输入：

`docElm.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT); `

这个只在chrome支持，其他浏览器不支持。firefox计划使用`requestFullscreenWithKeys`方法来支持鼠标输入，但是会触发用户通知已保证安全，firefox10以上，chrome 15和safari5.1以上都支持了。


## 4.页面可见性(`Page Visibility`)

页面可见性就是当前页面是处于显示状态还是隐藏状态，页面可见性对于网站的统计非常有用。有的时候我们会统计用户停留在每个页面的时间，这个时间就是：用户打开网页到网页关闭或者最小化之间的时间。

有的时候在视频播放的时候，当用户离开视频播放页面自动暂停视频播放，我们有时候也对那些定期刷新内容的页面进行控制，当该页面不可见则不刷新，可见则刷新。这些都是页面可见性的具体应用。

HTML5之前，我们可以监听 `onfocus() `事件。如果当前窗口得到焦点，那么我们可以简单认为用户在与该页面交互，如果失去焦点`onblur() `那么可以认为用户停止与该页面交互。

```JavaScript

// 当前窗口得到焦点
window.onfocus = function() {  // 动画
  // ajax 轮询等等
};
  // 当前窗口失去焦点
 window.onblur = function() { 
   // 停止动画
  // 停止 ajax 轮询等等
};
```

通过焦点来判断页面的可见性是非常不精确的，因为如果用户打开了网页，同时进行word内容的编辑，这个时候焦点在word编辑器里面，但是页面仍然可见。

### 4.1.可见性API和事件

首先，我们来看一下可见性的兼容性

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_11.png)

哈哈哈，只有Opera mini的浏览器都支持这个API，其他的浏览器都支持，这个可是非常不容易的。

可见性的属性：

* `document.hidden`：Boolean值，表示当前页面可见还是不可见

* `document.visibilityState`：返回当前页面的可见状态：

* `visible`：页面内容至少部分可见.意味着该页面是一个非最小化窗口的前台标签页.

* `hidden`：页面内容用户不可见.意味着当前浏览器窗口处于最小化模式,或者该页面是一个后台标签页.

* `prerender`：页面内容正在被预渲染,用户不可见(这种情况下document.hidden会返回true). 一个页面只有在初始化时可能为这个值, 一旦变为其他两种值,不可能再变回来.

* `unloaded`：当前文档已经被卸载,用户不可见(这种情况下document.hidden会返回true).

可见性的事件：

* `visibilitychange`: 当可见状态改变时候触发的事件。

### 4.2.可见性的使用

简单实例：

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<script>
			document.addEventListener('visibilitychange', function() {
				var isHidden = document.hidden;
				if(isHidden) {
					console.log('页面隐藏')
				} else {
					console.log('页面显示');
				}
			});
		</script>
	</body>
</html>
```

上述实例没有考虑浏览器的兼容性问题，我们就简单的测试一下，通过不停地切换其他页面与该页面，便可以触发该事件。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_12.png)
通过结果可以看到监听到了页面可见性的变化。


### 4.3.兼容浏览器的写法

```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<ul class="class1 class2 class3 ">
			<li onclick="launchFullScreen()">1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
		</ul>
		<script>
			function supportPageVisibility() {
				var hidden = "hidden",
					visibilityChange = "visibilitychange";
				if(typeof document.hidden !== "undefined") {
					hidden = "hidden";
					visibilityChange = "visibilitychange";
					state = "visibilityState";
				} else if(typeof document.mozHidden !== "undefined") {
					hidden = "mozHidden";
					visibilityChange = "mozvisibilitychange";
					state = "mozVisibilityState";
				} else if(typeof document.msHidden !== "undefined") {
					hidden = "msHidden";
					visibilityChange = "msvisibilitychange";
					state = "msVisibilityState";
				} else if(typeof document.webkitHidden !== "undefined") {
					hidden = "webkitHidden";
					visibilityChange = "webkitvisibilitychange";
					state = "webkitVisibilityState";
				} // 添加一个标题改变的监听器
				document.addEventListener(visibilityChange, function(e) {
					if(document[hidden]) {
						console.log('页面隐藏')
					} else {
						console.log('页面显示');
					}
				}, false);
			}
			supportPageVisibility();
		</script>
	</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_12.png)

## 5.预加载

网站优化一直是项目开发中的重点之中，常用的优化方式主要有：图片懒加载、图片sprite、css合并、js合并、数据本地存储、数据网络缓存等。这些都是项目中经常使用的，HTML5考虑到了这一点，提出了链接预加载的方法，其实，这个方案是FireFox提出的，所以它对链接预加载绝对的支持。


预加载是一种浏览器机制，使用浏览器空闲时间来预先下载\加载用户接下来很可能会浏览的页面/资源。页面提供给浏览器需要预加载的集合。 浏览器载入当前页面完成后，将会在后台下载需要预加载的页面并添加到缓存中。当用户访问某个预加载的链接时，如果从缓存命中,页面就得以快速呈现，说的直接一些就是让浏览器在后台提前下载一些文件。

### 5.1.link的prefetch属性

```JavaScript
  <!-- 页面，可以使用绝对或者相对路径 -->
 <link rel="prefetch" href="page2.html" />
  <!-- 图片，也可以是其他类型的文件 -->
  <link rel="prefetch" href="sprite.png" />
```

上面是预加载的使用方案，href就是预加载的文件，可以看到可以加载不通类型的文件。但是由于`prefetch`兼容性现在使用不是特别的多，我们来看一下兼容图：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_13.png)
可以看到兼容效果不是特别的好。考虑到prefetech的兼容，w3c提出了另外一个属性`dns-prefetch`属性。它的兼容性现在主流浏览器基本都支持。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_14.png)
通过上图可以看到`dns-prefetch`的兼容性比prefetch好很多。

### 5.2.link的`dns-prefetch`

<link rel="dns-prefetch" href="http://example-domain.com/">

可以看到使用方法和prefetch一样，只是rel的属性不一样。

### 5.3.注意事项

* 预加载可以跨域进行，当然，请求时cookie等信息也会被发送。

* 预加载可能破坏网站统计数据，而用户并没有实际访问。

* 浏览器兼容性不是很好

## 6. 页面监控(window.performance.timing)

百度的页面监控截图，在控制台输入`window.performance.timing`就可以看到。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_2.png)

### 6.1.各字段的含义：

* `navigationStart`：当前浏览器窗口的前一个网页关闭，发生unload事件时的Unix毫秒时间戳。如果没有前一个网页，则等于fetchStart属性。

* `unloadEventStart`：如果前一个网页与当前网页属于同一个域名，则返回前一个网页的unload事件发生时的Unix毫秒时间戳。如果没有前一个网页，或者之前的网页跳转不是在同一个域名内，则返回值为0。

* `unloadEventEnd`：如果前一个网页与当前网页属于同一个域名，则返回前一个网页unload事件的回调函数结束时的Unix毫秒时间戳。如果没有前一个网页，或者之前的网页跳转不是在同一个域名内，则返回值为0。

* `redirectStart`：返回第一个HTTP跳转开始时的Unix毫秒时间戳。如果没有跳转，或者不是同一个域名内部的跳转，则返回值为0。

* `redirectEnd`：返回最后一个HTTP跳转结束时（即跳转回应的最后一个字节接受完成时）的Unix毫秒时间戳。如果没有跳转，或者不是同一个域名内部的跳转，则返回值为0。

* `fetchStart`：返回浏览器准备使用HTTP请求读取文档时的Unix毫秒时间戳。该事件在网页查询本地缓存之前发生。

* `domainLookupStart`：返回域名查询开始时的Unix毫秒时间戳。如果使用持久连接，或者信息是从本地缓存获取的，则返回值等同于fetchStart属性的值。

* `domainLookupEnd`：返回域名查询结束时的Unix毫秒时间戳。如果使用持久连接，或者信息是从本地缓存获取的，则返回值等同于fetchStart属性的值。

* `connectStart`：返回HTTP请求开始向服务器发送时的Unix毫秒时间戳。如果使用持久连接（persistent connection），则返回值等同于fetchStart属性的值。

* `connectEnd`：返回浏览器与服务器之间的连接建立时的Unix毫秒时间戳。如果建立的是持久连接，则返回值等同于fetchStart属性的值。连接建立指的是所有握手和认证过程全部结束。

* `secureConnectionStart`：返回浏览器与服务器开始安全链接的握手时的Unix毫秒时间戳。如果当前网页不要求安全连接，则返回0。

* `requestStart`：返回浏览器向服务器发出HTTP请求时（或开始读取本地缓存时）的Unix毫秒时间戳。

* `responseStart`：返回浏览器从服务器收到（或从本地缓存读取）第一个字节时的Unix毫秒时间戳。

* `responseEnd`：返回浏览器从服务器收到（或从本地缓存读取）最后一个字节时（如果在此之前HTTP连接已经关闭，则返回关闭时）的Unix毫秒时间戳。

* `domLoading`：返回当前网页DOM结构开始解析时（即Document.readyState属性变为“loading”、相应的readystatechange事件触发时）的Unix毫秒时间戳。

* `domInteractive`：返回当前网页DOM结构结束解析、开始加载内嵌资源时（即Document.readyState属性变为“interactive”、相应的readystatechange事件触发时）的Unix毫秒时间戳。

* `domContentLoadedEventStart`：返回当前网页DOMContentLoaded事件发生时（即DOM结构解析完毕、所有脚本开始运行时）的Unix毫秒时间戳。

* `domContentLoadedEventEnd`：返回当前网页所有需要执行的脚本执行完成时的Unix毫秒时间戳。

* `domComplete`：返回当前网页DOM结构生成时（即Document.readyState属性变为“complete”，以及相应的readystatechange事件发生时）的Unix毫秒时间戳。

* `loadEventStart`：返回当前网页load事件的回调函数开始时的Unix毫秒时间戳。如果该事件还没有发生，返回0。

* `loadEventEnd`：返回当前网页load事件的回调函数运行结束时的Unix毫秒时间戳。如果该事件还没有发生，返回0。

 这些时间戳是告诉你这个网页从连接服务器开始，到连接服务器结束，到内容下载，到页面渲染，还有包括DNS，以及关闭浏览器时，卸载时所花费的时间.

### 6.2.如何分析页面整体加载速度

主要是查看指标值`PAGET_`页面加载时间,此指标指的是页面整体加载时间但不含(`onload`事件和`redirect`),此指标值可直接反应用户体验,从此项指标可以知道指定某时间段的页面加载速度值,以及和天,周,月的对比状况.

也可以查询指标`ALLT_`页面完全加载时间, 可以查询到从浏览器开始导航(用户点击链接或在地址栏输入url或点刷新,后退按钮)到页面onload 事件js完全跑完的所有时间.如果发现页面加载速度有增加或减少,则可以分项查询前面表格中的每个指标值,总的来说他们的关系如下:


* dom开始加载前所有花费时间=重定向时间+域名解析时间+建立连接花费时间+请求花费时间+接收数据花费时间

* `pageLoadTime页面加载时间`=域名解析时间+建立连接花费时间+请求花费时间+接收数据花费时间+解析dom花费时间+加载dom花费时间


* `allLoadTime页面完全加载时间`=重定向时间+域名解析时间+建立连接花费时间+请求花费时间+接收数据花费时间+解析dom花费时间+加载dom花费时间+执行onload事件花费时间


* `resourcesLoadedTime资源加载时间`=解析dom花费时间+加载dom花费时间


### 6.3.不支持HTML5元素的浏览器


可以利用getTime()获取当前的时间戳，完成某个功能之后再获取一个时间戳，两个时间戳相减就是完成这个功能所花费的时间。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232449/o_3.png)

不支持的浏览器只能通过这种办法粗略的计算该方法所花费的事件，其他的性能指标无法获得。





