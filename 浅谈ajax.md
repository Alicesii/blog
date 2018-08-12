---

title: 浅谈ajax
comments: true
date: 2018-06-12 10:16:37
categories: 博客列表
tags: AJAX
about:

---

> AJAX 即 Asynchronous Javascript And XML，不是一门的新的语言，而是对现有持术的综合利用。其本质是在HTTP协议的基础上以异步的方式与服务器进行通信。

## 1.XMLHttpRequest

浏览器内建对象，用于在后台与服务器通信(交换数据) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。(局部更新)

内建对象：浏览器已经封装好了该方法，用的时候只需要new一下就可以拿来使用了。

举例：我们先写请求行和请求主体

```javascri
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script type="text/javascript">
			//new一下，实例化
			var xhr=new XMLHttpRequest;
			//请求
			//请求行
			xhr.open('get','01.txt');
			//以get的方式请求，请求主体为空。
			//请求主体
			xhr.send(null);
		</script>
	</head>
	<body>
	</body>
</html>
```
我们可以再浏览器看一下结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_1.png)

可以明显的看到请求的地址和请求的方法。

注意：请求主体即使是空的，也要写着，这个是不能忽略的。

现在可以来写请求头了

```javascript
//请求头
xhr.setRequestHeader('Content-Type','text/html');
```
再来刷一刷浏览器的结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_2.png)

请求发送完了，这下我们该接收响应了。这下客户端的请求发送过来了，等到服务器空闲就去处理，本来说是客户端得一直等着服务器端的处理，其实客户端不用一直等着去干别的事情，可以设置一个监听事件`onreadystatechange`监听服务器端什么时候处理完这个请求，等服务器处理完之后客户端在接收，这样客户端就不用一直等着，这也就是异步请求的原理。

```javascript
	//接收响应
			xhr.onreadystatechange=function(){
				console.log(xhr.readyState);
				if(xhr.readyState==4&&xhr.status==200){
					var result=xhr.responseText;
					console.log(result);
				}
			}
```
`onreadystatechang`属性一直监听着服务器端的变化，我们可以检测一下

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script type="text/javascript">
			//new一下，实例化
			var xhr=new XMLHttpRequest;
			console.log(xhr.readyState);
			//请求
			//请求行
			xhr.open('get','01.txt');
			console.log(xhr.readyState);
			//请求头
			xhr.setRequestHeader('Content-Type','text/html');
			console.log(xhr.readyState);
			//以get的方式请求，请求主体为空。
			//请求主体
			xhr.send(null);
			//接收响应
			xhr.onreadystatechange=function(){
				console.log(xhr.readyState);
				if(xhr.readyState==4&&xhr.status==200){
					var result=xhr.responseText;
					console.log(result);
				}
			}
		</script>
	</head>
	<body>
		<p id="result"></p>
	</body>
</html>
```
在浏览器控制台查看结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_3.png)

readyState属性的所有取值以及它所使用的范围：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_4.png)

## 2.同步

ajax请求的方式是一个典型的异步请求的方式。我们现在可以看一下同步请求

`open()`方法中的第三个参数为true时，表示异步，为false时表示同步，默认为true。

举例：

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			var xhr=new XMLHttpRequest;
			xhr.open("get","02.php",false);
			xhr.setRequestHeader('Content-Type','text/html');
			xhr.send(null);
			console.log("我是同步消息");	
		</script>
	</head>
	<body>
	</body>
</html>
```
* 02.php

```php
<?php
echo '我是同步方式的';
sleep(10);
?>
```
大家可以在控制台看一下，等到10秒之后才会在控制台输出“我是同步消息”这句话，也就是说以同步的方式发送消息，客户端会一直在等待服务器做出响应，这样的效率会很低。

注意：有关于php的代码只有在php中运行才会有效，在测试前应该配置好基本的环境。

## 3.XMLHttpRequest传递数据

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			var xhr=new XMLHttpRequest;
			xhr.open("get","03.php?name=yang")
			xhr.setRequestHeader("Content-Type","text/html");
			xhr.send(null);
			xhr.onreadystatechange=function(){
				if(xhr.readyState==4&& xhr.status==200){
					var result=xhr.responseText;
                     document.getElementById("result").innerHTML=result;
					console.log(result);
				}
			}
		</script>
	</head>
	<body>
		<p id="result"></p>
	</body>
</html>
```
* 03.php

```php
<?php
echo $_GET['name'];
?>
```
运行结果：很明显在客户端已经获取到了这样的结果

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_6.png)

大家是不是很好奇为什么传递数据为什么要是这样的形式？把要传递的参数写在请求资源的后面。

`xhr.open("get","03.php?name=yang")`

原因：因为在get的请求方式中，浏览器在默认情况下就是把参数放在URL后面的,我们要遵从浏览器的默认行为。

那如果以post的方式该怎么传递数据呢？肯定是和get的方式有些区别的。

以post的形式的话，传递的数据是放在`send()`方法中的。

注意：当以post形式提交表单的时候，请求头里会设置`Content-Type:application/x-www-form-urlencoded`，以get形式时不需要请求主体。

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			var xhr=new XMLHttpRequest;
			xhr.open("post","04.php")
			xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
			xhr.send("name=yang&age=18");
			xhr.onreadystatechange=function(){
				if(xhr.readyState==4&& xhr.status==200){
					var result=xhr.responseText;
                     document.getElementById("result").innerHTML=result;
					console.log(result);
				}
			}
		</script>
	</head>
	<body>
		<p id="result"></p>
	</body>
</html>
```
* 04.php

```php
<?php
echo $_POST['name'].$_POST['age'];
?>
```
运行结果：![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_7.png)

## 4.XMLHttpRequest实现局部更新功能(表单事件提交)

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>

	</head>
	<body>
		<input type="text" id="text" />
		<input type="submit" value="发送" id="sub" />
		<p id="result"></p>
		<script>
			var btn = document.getElementById("sub");
			var test = document.getElementById("text");
			btn.addEventListener('click', function() {
				//点击的时候才去实例化XMLHttpRequest，而不是加载的时候实例化XMLHttpRequest
				var xhr = new XMLHttpRequest;
				xhr.open("post", "05.php");
				//因为是post请求的方式，所以一定要设置Content-Type
				xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
				xhr.send('text=' + test.value);
				xhr.onreadystatechange = function() {
					if(xhr.readyState == 4 && xhr.status == 200) {
						var result = xhr.responseText;
						document.getElementById("result").innerHTML = result;
						console.log(result);
					}
				}
			})
		</script>
	</body>
</html>
```
* 05.php

```php
<?php
echo '<strong>'.$_POST['text'].'</strong>';
?>
```
运行结果

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_8.png)

那我们来看一个小案例吧。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Ajax</title>
	<style>
		body {
			margin: 0;
			padding: 0;
			background-color: #F7F7F7;
		}
		.chatbox {
			width: 500px;
			height: 500px;
			margin: 0 auto;
			border: 1px solid #CCC;
			background-color: #FFF;
			border-radius: 5px;
		}
		.messages {
			height: 350px;
			padding: 20px 40px;
			box-sizing: border-box;
			border-bottom: 1px solid #CCC;
			overflow: scroll;
		}
		.messages h5 {
			font-size: 20px;
			margin: 10px 0;
		}
		.messages p {
			font-size: 18px;
			margin: 0;
		}
		.self {
			text-align: left;
		}
		.other {
			text-align: right;
		}
		.form {
			height: 150px;
		}
		.form .input {
			height: 110px;
			padding: 10px;
			box-sizing: border-box;
		}
		.form .btn {
			height: 40px;
			box-sizing: border-box;
			border-top: 1px solid #CCC;
		}
		.form textarea {
			display: block;
			width: 100%;
			height: 100%;
			box-sizing: border-box;
			border: none;
			resize: none;
			outline: none;
			font-size: 20px;
		}
		.form input {
			display: block;
			width: 100px;
			height: 30px;
			margin-top: 5px;
			margin-right: 20px;
			float: right;
		}
	</style>
</head>
<body>
	<div class="chatbox">
		<div class="messages">	
		</div>
		<div class="form">
			<div class="input">
				<textarea></textarea>
			</div>
			<div class="btn">
				<input type="button" value="发送">
			</div>
		</div>
	</div>
	<!--<script type="text/template">
		<div class="item self">
			<h5>我说</h5>
			<p>你好</p>
		</div>
		<div class="item other">
			<h5>对方说</h5>
			<p>你好</p>
		</div>
	</script>-->
	<script>
		var btn = document.querySelector('.btn');
		var message = document.querySelector('.messages');
		btn.onclick = function () {
			var xhr = new XMLHttpRequest;
			// 获取表单的DOM节点
			var input = document.querySelector('textarea');
			// 获取文本内容
			var val = input.value;
			// 通过一函数创建并接凑DOM节点
			var item = createMessage('self', val);
			// 将创建完成的DOM节点追加到消息盒子里
			message.appendChild(item);
			// 置空表单
			input.value = '';
			]
			/******************以下是Ajax*******************/
			// 发起请求
			xhr.open('post', 'chat.php');
			// 设置请求头
			xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
			// 发送到服务器
			xhr.send('message=' + val);
			// 监听并处理响应
			xhr.onreadystatechange = function () {
				if(xhr.readyState == 4 && xhr.status == 200) {
					// 接收返回的消息
					var result = xhr.responseText;
					// 通过一函数创建并接凑DOM节点
					var item = createMessage('other', result);
					// 将接凑好的DOM节点追加到消息盒子里
					message.appendChild(item);
				}
			}
		}
		// 动态创建DOM
		function createMessage(flag, text) {
			// 创建结点
			var item = document.createElement('div'),
				h5 = document.createElement('h5'),
				p = document.createElement('p');
			// 添加类
			item.classList.add('item');
			item.classList.add(flag);

			// 判断主体
			switch(flag) {
				case 'self':
					h5.innerText = '我说';
					break;
				case 'other':
					h5.innerText = '对方说';
					break;
			}
			// 插入文本
			p.innerText = text;
			// 追加节点
			item.appendChild(h5);
			item.appendChild(p);
			return item;
		}
	</script>
</body>
</html>

```
* chat.php

```php
<?php
	$messages = array(
		'你说啥？',
		'你也好',
		'你找我有啥事？',
		'我在吃饭！'
	);
	// 通过array_rand()随机获取数组下标
	// var_dump(array_rand($messages));
	echo $messages[array_rand($messages)];
	sleep(1);
?>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_9.png)

## 5.简单的总结

`XMLHttpRequest`：浏览器内建对象，用于在后台与服务器通信(交换数据) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。

下面是一个简单的例子：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_10.png)

由于XMLHttpRequest本质基于HTTP协议实现通信，所以结合HTTP协议和上面的例子我们分析得出如下结果：

### 5.1.请求

HTTP请求3个组成部分与XMLHttpRequest方法的对应关系

* 请求行

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_11.png)

* 请求头

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_12.png)

get请求可以不设置

* 请求主体

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_13.png)

注意书写顺序

### 5.2.响应

HTTP响应是由服务端发出的，作为客户端更应关心的是响应的结果。

HTTP响应3个组成部分与XMLHttpRequest方法或属性的对应关系。

由于服务器做出响应需要时间（比如网速慢等原因），所以我们需要监听服务器响应的状态，然后才能进行处理。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_14.png)

`onreadystatechange`是Javascript的事件的一种，其意义在于监听`XMLHttpRequest`的状态。

* 获取状态行（包括状态码&状态信息）

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_15.png)

* 获取响应头

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_16.png)

* 响应主体

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_17.png)

我们需要检测并判断响应头的MIME类型后确定使用`request.responseText`或者`request.responseXML`

### 5.3.API详解

* xhr.open() 发起请求，可以是get、post方式

* xhr.setRequestHeader() 设置请求头

* xhr.send() 发送请求主体get方式使用xhr.send(null)

* xhr.onreadystatechange = function () {} 监听响应状态

* xhr.readyState = 0时，UNSENT open尚未调用

* xhr.readyState = 1时，OPENED open已调用

* xhr.readyState = 2时，HEADERS_RECEIVED 接收到头信息

* xhr.readyState = 3时，LOADING 接收到响应主体

* xhr.readyState = 4时，DONE 响应完成

* xhr.status表示响应码，

* xhr.statusText表示响应信息，

* xhr.getAllResponseHeaders() 获取全部响应头信息

* xhr.getResponseHeader('key') 获取指定头信息

* xhr.responseText、xhr.responseXML都表示响应主体

注意：GET和POST请求方式的差异（面试题）

* GET没有请求主体，使用xhr.send(null)

* GET可以通过在请求URL上添加请求参数

* POST可以通过xhr.send`('name=itcast&age=10')`[键值对的形式]

* POST需要设置请求头：

```javascript
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

### 5.4.GET效率更好（应用多）

### 5.5.GET大小限制约4K，POST则没有限制。

## 6.XML数据VSJSON数据

问题：如何获取复杂数据呢？利用XML或者JSON，现在大多数情况下用的是JSON。我们可以先来看一下XML。

### 6.1.XML数据

XML是一种标记语言，很类似HTML，其宗旨是用来传输数据，具有自我描述性（固定的格式的数据）。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_18.png)

语法规则

* 必须有一个根元素

* 不可有空格、不可以数字或.开头、大小写敏感

* 不可交叉嵌套

* 属性双引号（浏览器自动修正成双引号了）

* 特殊符号要使用实体

* 注释和HTML一样

虽然可以描述和传输复杂数据，但是其解析过于复杂并且体积较大，所以实现开发已经很少使用了。

下面是一个关于XML的小案例

* index.html

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>XML</title>
		<style>
			table {
				width: 300px;
				border-collapse: collapse;
			}
			td {
				height: 40px;
				text-align: center;
				border: 1px solid #CCC;
			}
		</style>
	</head>
	<body>
		<table>
		</table>
		<div class="btn"><input type="button" value="获取数据"></div>
		<script>
			var btn = document.querySelector('.btn');
			btn.onclick = function() {
				var xhr = new XMLHttpRequest;
				xhr.open('get', 'stars.php');
				xhr.send(null);
				xhr.onreadystatechange = function() {
					if(xhr.readyState == 4 && xhr.status == 200) {
						// 一个新的属性，用来接收XML格式的数据
						var result = xhr.responseXML;
						// console.log(result);
						//解析XML文档的内容
						var items = result.getElementsByTagName('root')[0];
						//console.log(items);
						var item = items.getElementsByTagName('item');
						 //console.log(item);
						 //<item>的子元素有多少个，就循环遍历几次(4)
						var html = '';
						for(var i = 0; i < item.length; i++) {
							var name = item[i].getElementsByTagName('name')[0];
							var photo = item[i].getElementsByTagName('photo')[0];
							var album = item[i].getElementsByTagName('album')[0];
							var age = item[i].getElementsByTagName('age')[0];
							var sex = item[i].getElementsByTagName('sex')[0];
							var nameText = name.firstChild.nodeValue;
							var photoText = photo.firstChild.nodeValue;
							var albumText = album.firstChild.nodeValue;
							var ageText = age.firstChild.nodeValue;
							var sexText = sex.firstChild.nodeValue;
							html += '<tr>';
							html += '<td>' + nameText + '</td>';
							html += '<td>' + albumText + '</td>';
							html += '<td>' + ageText + '</td>';
							html += '<td>' + sexText + '</td>';
							html += '</tr>';

						}
						document.querySelector('table').innerHTML = html;
					}
				}
			}
		</script>
	</body>
</html>
```
* stars.php

```php
<?php
	// 指定文档类型
	header('Content-Type:text/xml; charset=utf-8');

	$result = file_get_contents('stars.xml');

	echo $result;
?>
```
* stars.xml

```
<?xml version="1.0" encoding="utf-8"?>
<root>
	<item>
		<name type="star">王力宏</name>
		<photo>./images/wlh.jpg</photo>
		<album>&lt;&lt;改变自已&gt;&gt;</album>
		<age>39</age>
		<sex>男</sex>
	</item>
	<item>
		<name type="star">周杰伦</name>
		<photo>./images/wlh.jpg</photo>
		<album>&lt;&lt;我很忙&gt;&gt;</album>
		<age>39</age>
		<sex>男</sex>
	</item>
	<item>
		<name type="star">王力宏</name>
		<photo>./images/wlh.jpg</photo>
		<album>&lt;&lt;改变自已&gt;&gt;</album>
		<age>39</age>
		<sex>男</sex>
	</item>
	<item>
		<name type="star">王力宏</name>
		<photo>./images/wlh.jpg</photo>
		<album>&lt;&lt;改变自已&gt;&gt;</album>
		<age>39</age>
		<sex>男</sex>
	</item>
</root>
```
运行结果：![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/t_19.png)

大家可以看出来真的是用xml来表示比较复杂的数据时，解析数据时真的是超级麻烦并且文档体积也很大。我们可以用JSON来表示复杂的数据，解析数据的时候也较为简单。

### 6.2.JSON：即JavaScript Object Notation，另一种轻量级的文本数据交换格式，独立于语言。

语法规则：

* 数据在名称/值对中

* 数据由逗号分隔(最后一个健/值对不能带逗号)

* 花括号保存对象方括号保存数组

* 使用双引号

我们可以看下JSON解析数据的时候是否也和XML解析数据时一样复杂？

* index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>JSON</title>
	<style>
		table {
			width: 300px;
			border-collapse: collapse;
		}
		td {
			height: 40px;
			text-align: center;
			border: 1px solid #CCC;
		}
	</style>
</head>
<body>
	<table>
	</table>
	<div class="btn"><input type="button" value="获取数据"></div>
	<script>
		var btn = document.querySelector('.btn');
		btn.onclick = function () {
			var xhr = new XMLHttpRequest;
			xhr.open('get', 'stars.php');
			xhr.send(null);
			xhr.onreadystatechange = function () {
				if(xhr.readyState == 4 && xhr.status == 200) {
					var result = xhr.responseText;
					console.log(typeof(result));
					//将JSON格式的字符串类型转化为JavaScript对象
					var data = JSON.parse(result);
					console.log(data);
					var html = '';
					for(var i=0; i<data.length; i++) {
						html += '<tr>';
						html += '<td>' + data[i].name + '</td>';
						html += '<td>' + data[i].ablum + '</td>';
						html += '<td>' + data[i].age + '</td>';
						html += '<td>' + data[i].sex + '</td>';
						html += '</tr>';
					}
					document.querySelector('table').innerHTML = html;
				}
			}
		}
	</script>
</body>
</html>
```
* stars.json

```
[
	{
		"name": "王力宏",
		"photo": "./images/wlh.jpg",
		"ablum": "<<改变自已>>",
		"age": 39,
		"sex": "男"
	},
	{
		"name": "王力宏",
		"photo": "./images/wlh.jpg",
		"ablum": "<<改变自已>>",
		"age": 39,
		"sex": "男"
	},
	{
		"name": "王力宏",
		"photo": "./images/wlh.jpg",
		"ablum": "<<改变自已>>",
		"age": 39,
		"sex": "男"
	},
	{
		"name": "王力宏",
		"photo": "./images/wlh.jpg",
		"ablum": "<<改变自已>>",
		"age": 39,
		"sex": "男"
	}
]
```
* stars.php

```
<?php
	// 指定文档类型
	header('Content-Type:application/json; charset=utf-8');
	$result = file_get_contents('stars.json');
	echo $result;
?>
```

运行结果:![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/t_19.png)

`parse()`方法的作用：将JSON格式的字符串类型转化为JavaScript对象

为什么要转化呢？

我们应该都知道不同的语言都有着自己的一套语法规则，比如说不同语言定义变量、定义数组、定义对象的方式肯定都有所不同，所以不同的语言之间的通信是通过字符串来表示的，比如说JSON格式的字符串，用""双引号包起来的不管是对象也好，数组也好，在不同的语言都是可以运行通过的，并不会报错，当各个不同的语言接收到了这个字符串之后，就利用自己所对应语言所封装的方法转化成自己可以识别的对象，以便后续的操作，比如说在JavaScript预言中，将JSON格式的字符串类型转化为JavaScript对象的方法是`parse()`。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_20.png)

现在我们可以得到一个毫无疑问的答案，JSON格式的数据要比XML格式的数据简单的多，我们以后还是尽量用JSON格式的数据，XML格式的数据已经淘汰了呢。


我们得需要了解一下XMLHttpRequest在标准浏览器和IE浏览器存在着兼容性问题，我们需要一个兼容性的写法

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_21.png)