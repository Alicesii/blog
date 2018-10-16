---
title: jQuery对象和DOM对象
comments: true
date: 2018-06-27 10:05:32
categories: 前端
tags: JavaScript框架

---
> JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，是一门十分强大的语言，Jquery是一个基于JavaScript的框架，它们之间有着千丝万缕的关系，我们一起来了解下吧。

## JavaScript VS JQuery

```javascript

 <body>
	<input type="button" id="demo" value="我是按钮" />
	<div></div>
	<div></div>
	<div></div>
</body>
	<script type="text/javascript">
        window.onload = function() {
            document.getElementById("demo").onclick = function() {
                // 要实现的效果：
                // 1.让div展示出来
                // 2.给div一段文本内容
               var divs = document.getElementsByTagName("div");
                //获取一个数组
                for(var i = 0; i < divs.length; i++) {
                    divs[i].style.display = "block";
                    divs[i].innerText = "我是文本内容";
                }
            };
        };
</script>
```

这是用JavaScript写的，功能是点击一个按钮，就会把隐藏的div显示出来，并且给每个div添加一段文本内容。但是这段用js写的代码存在一个很大的问题。

**问题1：**

```javascript
<script type="text/javascript">

            window.onload = function() {
				console.log(1);
			}
			window.onload = function() {
				console.log(2);
			}
			window.onload = function() {
				console.log(3);
			}
			window.onload = function() {
				console.log(4);
			}
</script>
```

如果页面存在很多个onload函数，会有什么后果呢？

运行结果：4


所以说结果是弹出最后一个onload，window.onload存在事件覆盖的问题，也就是说第四个window.onload将前三个window.onload函数给覆盖了。


**问题二:**

```

		<script type="text/javascript">

		   window.onload = function() {
				document.getElementById("demo1").onclik = function() {

				};
               document.getElementById("demo2").onclick=
               function(){

               };
			};
		</script>
```
   若绑定的"demo1"的id值不存在，则打开浏览器的控制台会看到错误的消息，不再执行下去，所以会影响绑定的"demo2"中功能的执行，代码的容错率太低，对用户来说不希望是这样的。

**问题三:各大主流浏览器的兼容性问题：**

`innerText`属性支持谷歌浏览器。不支持火狐浏览器，利用`innerHtml`属性存在着各种兼容性问题。

**总结：**

* `window.onload`事件只出现一次，如果出现多次，后面的事件会覆盖掉前面的事件

* 代码的容错行为，我们不希望看到错误

* 简单功能实现很繁琐，例如渐变的动画效果。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_1.png)


## 了解JQuery


**jQuery和JS库的联系与区别**

jQuery其实是js的一个库（框架），它封装了我们开发中常用的一些功能，方便我们调用，提高了我们的开发效率，jQuery库：是别人帮我们封装好的js库

Js库，它把我们常用的功能放到单独一个文件中，用的时候直接拿到我们的页面中来就可以了。


```javascript

	<script src="../js/jquery.min.js"></script>
	<script type="text/javascript">
    $(document).ready(function(){
    	$("#demo").click(function(){
    		$("div").show(1000).html("这是一段文本");
    	});
    });
	</script>

```

大家可以清晰地看到用jQuery实现同样的效果比用原生的javascript见得到的多了。并且不存在兼容性问题、不存在覆盖问题。这个大家不相信的话可以自己试一下。

```javascript

	<script src="../js/jquery.min.js"></script>
	<script type="text/javascript">
    $(document).ready(function(){
    	console.log(1);
    });
$(document).ready(function(){
    	console.log(2);
    });    
$(document).ready(function(){
    	console.log(3);
    });    
$(document).ready(function(){
    	console.log(4);
    });    
	</script>
```

输出结果：

![ ](https://www.cnblogs.com/images/cnblogs_com/cliy-10/1232432/o_2.png)

大家这下相信相信不会存在覆盖问题了吧，应该也体会到了jQuery的强大了。


JQuery还有一个非常牛逼的地方就是即使你代码中某些部分写错了，真正的效果在浏览器执行不出来，但是在浏览器的控制台中你不会看到错误，如果控制台没有报错，我们调试的时候要怎么办呢，这就需要我们掌握一定的调试jQuery代码的技巧。

**jquery的优势**

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_3.png)

**jQuery核心理念**

* wirte less,do more

* 链式编程、隐式迭代。

**jquery基本使用步骤:**

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_4.png)

**jquery特点：**

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_5.png)

* `Lightweight Footprint`：非常轻量级，其实压缩版源文件有80KB~90KB之间，但是官网上说的是30kb,这是什么原因呢？因为gzip,当浏览器真正加载的时候会经过http请求，这时候只要服务器开启gzip压缩，它的体积就能缩小到30KB，它同时支持AMD规范(模块化开发、按需加载)。

* `css3 Compliant`：支持所有css3的选择器，降低了学习的难度。

* `Cross-Browser`：跨浏览器兼容、IE6版本以上的浏览器都支持。


**注意:**

* $ === jQuery

* $代表了我们jQuery的对象，只要操作jQuery就可以用$开始。

* jQuery占用了$和jQuery这两个变量，我们不要再去声明这两个变量，否则会报错。

* Js命名规范：数字、字母、下划线.

* 如果自己用var声明一个$符号，那jQuery的函数将不起作用



## JQuery入口函数

```javascript
//第一种写法：
$(document).ready(function(){

	});

//第二种写法：一般不使用

jQuery(document).ready(function(){

	});
//第三种写法

$(function(){

	});

//第四种写法

$(window).load(function(){


    });

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_10.png)


**jQuery的入口函数与JavaScript的入口函数区别**

* window.onload等到所有的外部资源加载完毕之后才执行，外部资源包括外部的图片文件、css文件、js文件、视频文件、音频文件等等......


* 让一些不同的线程去加载那些外部的文件，jQuery是在主进程加载完之后就立即执行，也就是说jQuery不用等到所有的外部文件都加载完成才去执行，而那些外部文件就交给线程来执行。也这样理解，jQuery是使得一旦页面的文档对象模型（DOM树）变为安全的操纵，就立即运行JavaScript代码。

* jquery中ready()函数的执行时机要比JavaScript中window.onload函数的时间要早。



* jquery中这样的加载机制也存在着一定的问题，比如说你想要获取一张图片的高度时，可能就获取不到，因为可能这时候你想获取的图片的还没被加载进来，如果想要获取图片的高度，还必须放在window.onload中。

* 在JQuery和中JavaScript存在着这样的一个恒等式,所以如果在JQuery要获取图片的高度，必须要将获取图片宽度或者高度的代码放在这样的入口函数中。

`$(window).load(function(){}) === window.onload`


## jquery对象与DOM对象

**jquery对象与DOM对象之间的区别**

```javascript
<input type="button" value="我是按钮"  id="btn1"/>
    <script type="text/javascript">
	 //javaScript方式获取按钮的id
	    window.onload=function (){
		  document.getElementById("btn1").onclick=function(){
			}
		 }
	 //jQuery方式获取按钮的id值
		$(document).ready(function(){
		   $("#btn1").click(function(){
		  })
	    })
</script>
```

问题：为什么一个是JavaScript调用onclick()方法而jquery调用click()方法？

因为jQuery中只有click方法，没有onclick方法，并且click方法和onclick方法功能相同，却更加简单。

JavaScript中既有onclick()方法又有click()方法，但是这两个方法有着稍微的区别。

```javascript
<input type="button" value="我是按钮"  id="btn1"/>
<input type="button" value="我是按钮"  id="btn2"/>

//实现的功能：javaScript方式:用第二个按钮去触发第一个按钮的事件
  window.onload = function() {
	 document.getElementById("btn1").onclick = function (){
		 console.log("我是第一个按钮")
			}
	 document.getElementById("btn2").onclick = function() {
		  document.getElementById("btn1").click();
			}
		}
</script>
```

用第二个按钮的事件去触发第一个按钮的事件，这就是javascript对象中click方法的作用。

注意：jQuery中所有的事件名称都不带"on",原生的javaScript中既有带"on"的事件也有不带"on"的事件。

```javascript

<script type="text/javascript">
			$(document).ready(function(){
			var $btn=$("#btn1");
			console.log($btn);
			});
			window.onload=function(){
			var btn=document.getElementById("btn2")
			console.log(btn);
			}
			</script>
```

运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_6.png)

大家根据运行结果应该能清晰地看出jquery和DOM对象的区别了吧。

**jquery对象与DOM对象的互相转换**

把jQuery对象转化为DOM对象的主要用途就是为了使用DOM对象中的某些方法。也就是说，当jquery对象转化成DOM对象之后，就可以调用带"on"的事件了，比如说onclick事件、onchange事件等。

```javascript
			$(document).ready(function(){
               //jQuery对象转化为DOM对象
                //第一种方式
			$("btn1")[0].onclick=function() {
				console.log("我是jQuery对象转化来的");
			}	
			   //第二种方式
			$("btn1").get(0).onclick=function() {
				console.log("我是jQuery对象转化来的");
			}
		
			});
			</script>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_9.png)

问题1：为什么是这样形式的转换？

原因：jQuery实际上把选取到的DOM对象转化成一个数组，放在自己的一个集合中，然后封装成了一个jQuery对象，把选择的第一个元素就放到了第一个位置，其实获取到的对象是一个**类数组**，并不是纯粹的数组。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_7.png)

问题2：那数组中取值为什么是0？

因为通过id选择器获得只有唯一的一个，只需要转化一个，所以只取数组的第一个值就行了，下为标0。如下代码

```
<script type="text/javascript">

$(document).ready(function(){

        $("btn1")[0].onclick=function(){};//等价
            //等价
	    var $btn=[document.getElementById("btn1")]; //类数组，下标从0开始
	    $btn[0].onclick=function(){
	    }
});
</script>
```


注意，因为不仅仅是存在着id选择器、还有标签选择器、还有类选择器，通过这类的选择器一次性可以获取多个，如下图，jquery内部实现机制并没有办法一次性将全部的jquery对象转化为DOM对象，jQuery实际上把选取到的某一个DOM对象转化成一个数组。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232432/o_8.png)


## jQuery绑定事件

```
<script type="text/javascript">

//第一种写法：
$(document).ready(function(){
    事件源.事件(function(){
        事件处理程序
        });
    });
//第二种写法：
$(function(){
      事件源.事件(function(){
         事件处理程序
        });
    });
</script>
```


## jQuery版本问题

* jQuery 1.xxx和jQuery 2.xxx版本之间的区别：

 jQuery2版本的体积更小，并且不再支持IE6,7,8浏览器，从IE9浏览器以上开始支持。



