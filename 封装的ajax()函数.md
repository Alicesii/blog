---
title: ajax()函数的封装
comments: true
date: 2018-06-15 18:33:45
categories: 博客列表
tags: AJAX
about:

---


## 1.封装一个自己的`ajax()`方法,减少代码的重复，不然每次都要重复的写一些相同的代码。

一般情况下，我们如果要把一个功能封装成一个函数，应该怎么去构想呢？？？

我们首先需要确定这个函数有几个参数？

`url,get/post,data`我们可能需要这三个参数

正常的思路：

```
function ajax(url,type,data,callback){

}
```
需要一个回调函数来执行获取到数据之后的操作。

但是我们又嫌弃函数的参数名字太长了，我们可以改成这样的形式

```
var obj={
	url:'',
	type:'',
	data:'',
	callback:function(){}
}
function ajax(options){

}
ajax(obj); //调用ajax()函数
```
这种思路往往是被采用的，和第一种写法比较,有什么好处呢？

我们可以看出第二种方法明显的更简洁一些，并且第一种方式的参数的位置是被固定死的，第二种方式参数的位置可以很灵活。

我们还有一个问题，如果你写了一个函数为`function ajax(){},`但是你的同事并不知道你占用了这个名字，它也写了个函数叫`function ajax(){}`,如果这两个函数加载到同一个页面，其中的一个肯定会被覆盖的，我们怎么解决这个问题呢？怎么解决命名冲突问题呢？

```
//命名冲突
function ajax(){}


function ajax(){}
```

解决办法：我们把函数封装到一个对象内部中

```javascript
var obj={
	ajax:function(){}
}

obj.ajax()  //调用ajax()方法

//另一个同事
var obj1={

	ajax:function(){}
}
obj1.ajax()  //调用ajax()方法
```

这样就解决了函数命名冲突问题.(把函数封装在一个对象内部)，专业一点来说就是采用命名空间的方式来解决函数命名冲突问题。

**怎么使用命名空间？：定义一个全局的对象，把方法绑到这个全局对象的下面。**

我们现在就可以动手来写了：

所需要的参数：`url,get/post,data`

```
var $ = {
    //处理data的格式
	params: function(data) {
	    var res='';
	    for(var key in data){
	    res += key+ '=' +data[key]+ '&';
	   }
	  return res.slice(0,-1);
	},
	ajax: function(options) {
		//传参数
		//传给函数的地址||默认的当前地址
		var url = options.url || location.pathname;
		//传给函数的请求方法||默认的请求方式
		var type = options.type || 'get';
		//定义一个方法params()处理数据格式
		var data = this.params(options.data);
	}
}
```
经过以前的案例学习，我们可以知道经过post方式发送的数据的格式一般是`xhr.send("name=yang&age=18")`这样的，但是调用的时候我们是这样的`data:{name:Asici,age:18}`,这样的形式是不匹配的，所以在ajax()函数中我们需要处理一下data数据的格式。

函数的调用

```
$.ajax({
	url: '01.php',
	type: 'post',
	data: {
		name: 'Asic',
		age:  '18'
	},
	success: function(data) {
		console.log(data);
	},
	error: function(err) {

    }
})
```
我们现在封装的`params()`可以达到我们的效果吗？

```JavaScript
params: function(data) {
	    var res='';
	    for(var key in data){
	    res += key+ '=' +data[key]+ '&';
	   }
	  console.log(res);
	}
```
![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_1.png)

通过上图我们可以发现最后多了一个&符号,还是不太符合要求，我们需要在修改一下。

```JavaScript
params: function(data) {
	    var res='';
	    for(var key in data){ 
	    res += key+ '=' +data[key]+ '&';
	   }
	  return res.slice(0,-1);
   console.log(res.slice(0,-1));
}
```
![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_2.png)

这样就达到了我们想要的效果了。

封装好了所需要的函数，现在就需要实例化了。

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			var $ = {
				//格式化参数
				params: function(data) {
					var res = '';
					for(var key in data) {
						res += key + '=' + data[key] + '&';
					}
					return res.slice(0,-1);
				},
				ajax: function(options) {
					//传参数
					//获取请求的地址||默认的当前地址
					var url = options.url || location.pathname;
					//获取请求方式||默认的请求方式
					var type = options.type || 'get';
					//定义一个方法params()处理数据格式(格式化参数)
					var data = this.params(options.data);
					//实例化XMLHttpRequest
					var xhr=new XMLHttpRequest;
					//判断以get形式请求
					if(type=='get'){
						url = url + '?' + data;
						data=null;
					}
					//发起一个请求
                   xhr.open(type,url);
                    //以post形式的时候需要设置文档类型
                   if(type=='post')
                   {
                   	  xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
                   }
                   //发送请求主体
                   xhr.send(data);
				}
			}
			$.ajax({
				url: '01.php',
				type: 'post',
				data: {
					name: 'Asic',
					age: '18'
				},
				success: function(data) {
					console.log(data);
				},
				error: function(err) {}
			})
		</script>
	</head>
	<body>
	</body>
</html>
```
基本的封装已经好了，就只差事件监听了，我们先在浏览器中测试一下，看目前的封装有没有问题。

post请求方式：

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_3.png)

get请求方式

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_4.png)

大家可能不太知道`location.pathname`是什么，我来说一下

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_5.png)

`location`对象有很多的属性，`location.pathname`是获取当前页面的地址信息，还有很多其他属性，大家都可以看下。

我们接下来就要封装`onreadystatechange()`方法

```javascript
 xhr.onreadystatechange=function(){
     if(xhr.readyState==4&&xhr.status==200){
          }
    }
```

在`XMLHttpRequest`中有这样一些方法：

```JavaScript

console.log(xhr.getAllResponseHeaders())

console.log(xhr.getResponseHeader('content-type'));
```

* `getAllResponseHeaders`:获取响应的所有http头

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_6.png)

* `getResponseHeader`：从响应信息中获取指定的http头

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_7.png)

* `xhr.responseText`
 
* `xhr.responseXML`


因为可能获取到不同类型的数据所以我们要分情况对待,我们需要知道当前返回的到底是什么类型的数据，一般情况下我们都希望返回的是`JSON`数据(字符串)，继续封装吧。

完整的封装版。

* `ajax.js`

```javascript
var $ = {
	params: function(data) {
		var res = '';
		for(var key in data) {
			res += key + '=' + data[key] + '&';
		}
			return res.slice(0,-1);
	},
	ajax: function(options) {
		//传参数
		//传给函数的地址||默认的当前地址
		var url = options.url || location.pathname;
		//传给函数的请求方法||默认的请求方式
		var type = options.type || 'get';
		//定义一个方法params()处理数据格式
		var data = this.params(options.data);
		var xhr=new XMLHttpRequest;
			if(type=='get'){
				url = url + '?' + data;
				data=null;
			}
                xhr.open(type,url);
                if(type=='post')
                   {
                   	 xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
                   }
                xhr.send(data);
                xhr.onreadystatechange=function(){
                    if(xhr.readyState==4&&xhr.status==200){
                    var ct=xhr.getResponseHeader('content-type');
                     //indexOf()判断ct中是不是有json这个字符串，返回值为该字符串的起始位置。
                    if(ct.indexOf('json')!=-1){
                    //将JSON格式的字符串转化成JavaScript对象parse()
                    var data=JSON.parse(xhr.responseText); 
                    //接下来就要交给回调函数工作了
                    options.success(data);
                 }else{
               options.error()
            }
         }
       }
	}
}
```

* `01.php`

```
<?php
	header('Content-Type:application/json; charset=utf-8');
	echo json_encode($_REQUEST);
?>
```
测试：

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script src="ajax.js"></script>
		<script>
			$.ajax({
				url: '01.php',
				type: 'post',
				data: {
					name: 'Asic',
					age: '18'
				},
				success: function(data) {
					console.log(data);
				},
				error: function(err) {}
			})
		</script>
	</head>
	<body>
	</body>
</html>
```

测试结果：

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_8.png)

缺点：到目前为止，这个封装的小工具其实只考虑了部分情况，比如说数据格式只能是`JSON`形式的，在实际情况中，其实还可能是`XML`数据格式的，这种情况其实我们都没有考虑，只是把一些常用的一些操作封装了，许多地方还有优化的可能，这里先不说了。

那我们来测试一下，我们封装的这个到底好用不好用。

* index.html

```javascript
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
	<script src="ajax.js"></script>
	<script>
		$.ajax({
				url: 'stars.php',
				type: 'post',
				data: {
					test:'test ajax tools'
				},
				success: function(data) {
					console.log(data);
				},
				error: function(err) {}
			})
	</script>
</body>
</html>
```

* `stars.json`

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

* `stars.php`

```
<?php
	// 指定文档类型
	header('Content-Type:application/json; charset=utf-8');
	$result = file_get_contents('stars.json');
	echo $result;
?>
```
运行结果:

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_9.png)

![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_10.png)

说明我们的封装基本是正确的，那我们利用自己封装的函数看能不能实现和以前一样的效果。

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
		<script src="ajax.js"></script>
		<script>
			$.ajax({
				url: 'stars.php',
				type: 'post',
				data: {
					test: 'test ajax tools'
				},
				success: function(data) {
					var html = '';
					for(var i = 0; i < data.length; i++) {
						html += '<tr>';
						html += '<td>' + data[i].name + '</td>';
						html += '<td>' + data[i].ablum + '</td>';
						html += '<td>' + data[i].age + '</td>';
						html += '<td>' + data[i].sex + '</td>';
						html += '</tr>';
					}
					document.querySelector('table').innerHTML = html;
				},
				error: function(err) {}
			})
		</script>
	</body>
</html>
```
![](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_11.png)

是不是很开心我们自己封装的函数可以实现相同的效果。

