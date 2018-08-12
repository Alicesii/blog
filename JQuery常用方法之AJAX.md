---
title: jQuery常用方法之AJAX
comments: true
date: 2018-05-06 20:55:22
categories: 博客列表
tags: JavaScript框架
about:

---

> 我们不得不承认一点我们自己封装的`ajax()`函数有各种各样的Bug，那我们可以看下封装JQuery的大牛封装的`ajax()`方法是有多么的厉害，站在巨人的肩膀上总会看的更远。

## JQuery版的表单事件提交

* jQuery.html

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

		.ajax {
			width: 400px;
			text-align: center;
			margin: 50px auto;
		}
	</style>
</head>
<body>
	<div class="ajax">
		<input type="text" id="text" />
		<input type="submit" value="发送" id="sub" />
		<p id="result"></p>
	</div>
	<script src="js/jquery.min.js"></script>
	<script>
		$('#sub').on('click', function () {
			var val = $('#text').val();
			$.ajax({
				url: 'jquery.php',
				type: 'post',
				data: {text: val},
				success: function (data) {
					console.log(data);
					$('#result').html(data);
				},
				error: function () {
					console.log('错误啦');
				},
			});
		});
	</script>
</body>
</html>
```
* jQuery.php

```
<?php
echo '<strong>'.$_POST['text'].'</strong>';
?>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240733/o_8.png)

我们明显的可以看到JQuery的方式比利用原生的JavaScript要方便的多。

首先看一下JQuery中关于ajax都封装了哪些方法，

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_12.png)

看到这么多的方法是不是都要发怵了，不过我们只需要掌握常用的方法就好了。

## ajax->请求

* `$.ajax(url,[settings])`

* `load(url,[data],[callback])`

* `$.get(url,[data],[fn],[type])`

* `$.getJSON(url,[data],[fn])`

* `$.getScript(url,[callback])`

* `$.post(url,[data],[fn],[type])`

* `$.ajaxSetup([options])`

* `serialize()`：简单的来说就是把表单的数据转化成键值对key:value的形式。

举例：

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

		.ajax {
			width: 400px;
			text-align: center;
			margin: 50px auto;
		}
	</style>
</head>
<body>
	<div class="ajax">
		<input type="text" id="text" />
		<input type="submit" value="发送" id="sub" />
		<p id="result"></p>
	</div>
	<script src="./js/jquery.min.js"></script>
	<script>
		$('#sub').on('click', function () {
			var val = $('#text').val();
			$.ajax({
				url: 'jquery.php',
				type: 'post',
				// 约束服务器返回的数据格式
				dataType: 'json',
				// 设置超时
				timeout: 5000,
				data: {text: val},
				beforeSend: function () {
					// 通常用来做验证工作、Loading
					console.log('提前发送');
				},
				success: function (data) {
					console.log(data);
					$('#result').html(data);
					console.log(1);
				},
				error: function () {
					console.log('错误啦');
				},
				// 响应完成时调用，执行顺序晚于success 或者 error
				complete: function () {
					console.log(2);
				}
			});
		});
	</script>
</body>
</html>
```

### 解释一：`beforesend:function(){}：`发送之前执行的函数

使用场景：

* 比如说在提交表单数据之前，总是希望验证一下。

* 还有就是loading加载状态。

### 解释二：`dataType: `约束服务器返回的数据格式

* `dataType: 'json'`：程序继续往下执行

* `dataType: 'xml'`：数据的格式不正确，执行错误函数。(因为请求的是JSON类型的数据)

```JavaScript
 error: function () {
	console.log('错误啦');
}
```
### 解释三：`timeout`:3000

设置超时处理，当超过3秒钟后，就中断请求，不在等待了。

### 解释四：`complete: function(){}`

完成响应之后指定的函数，也就是说不管是完成成功的响应，还是完成失败的响应，反正就是最后需要执行的函数。

### 解释五：`serialize()`

简单的来说就是把表单的数据转化成键值对`key:value`的形式

### 解释六：`$.ajaxSetup()`：用来全局设置的，类似于全局变量的效果(全局===局部)

关于JQuery封装的`ajax()`方法配置的回调函数还有很多，我们就了解一下平常能用到的就可以了。

举例：表单注册

* index.html

```javascript
<body>
	<div class="register">
		<form id="ajaxForm">
			<ul>
				<li>
					<label for="">用户名</label>
					<input type="text" name="name" class="name">
				</li>
				<li>
					<label for="">请设置密码</label>
					<input type="password" name="pass" class="pass">
				</li>
				<li>
					<label for="">请确认密码</label>
					<input type="password" name="repass" class="repass">
				</li>
				<li>
					<label for="">验证手机</label>
					<input type="text" name="mobile" class="mobile">
				</li>
				<li>
					<label for="">短信验证码</label>
					<input type="text" name="code" class="code">
					<input type="button" value="获取验证码" class="verify">
				</li>
				<li>
					<label for=""></label>
					<input type="button" class="submit" value="立即注册">
				</li>
			</ul>
		</form>
	</div>
	<div class="tips">
		<p></p>
	</div>
	<script src="js/jquery.min.js"></script>
	<script>
		//点击提交表单
		$('.submit').on('click',function(){
			var that=$(this);
			if(that.is('.disabled')) return;
		//获取表单文本框的值，可以用到serialize()方法，
		//把表单的数据转化成键值对key:value的形式，这种格式的数据正是我们所需要的。
			var value=$("#ajaxForm").serialize();
			$.ajax({
				url:"register.php",
				type:"post",
				dataType:"json",
				data: value,
				beforeSend:function(){
					if($('.name').val()==''){
						$('.tips p').text('用户名不能为空').show().delay(1500).fadeOut();
					       //终止请求
				       	return false;
					}
					//添加禁止项
					that.addClass("disabled");
					that.val("正在提交");
				},
				success:function(data){
					//在接口中约定了注册成功的code为10001，注册失败的code为10002
					if(data.code!=10000){
						alert('注册失败');
						that.val('注册失败请重试');
						return false;
					}else{//注册成功
						location.href="https://yingy0.github.io/";
					}
				},
				error:function(){
				},
				complete:function(){
					that.removeClass("disabled");
				}
			})
		})
	</script>
</body>
```
* register.php

```
<?php
	if($_POST['name']) {
		//只要name这个表单项填写了内容，即认为注册成功,
		//不去复杂的判断密码的格式、用户名的格式，减轻服务端的压力
		//但是如果填name这个表单项，则认为注册失败
		$arr = array(
			'code'=>10000,
			'msg'=>'注册成功',
			'result'=>array('name'=>'yangying', 'age'=>18)
		     //具体返回的结果可能又是一个数组
		);
	} else {
		$arr = array(
			'code'=>10002,
			'msg'=>'注册失败',
			'result'=>'一些信息'
		);
	}
	echo json_encode($arr);
	sleep(3);
?>
```
### 解释一：获取表单文本框的值：

一般情况下我们用的是val()方法获取文本框的值，但是每次只能获取一个表单文本框的值，如果我们需要所有的表单文本框的数据，那我们就得写好多个重复的代码，这样是很繁琐的。

举例

```<li>
	<label for="">用户名</label>
	<input type="text" name="name" class="name">
</li>
<li>
	<label for="">请设置密码</label>
	<input type="password" name="pass" class="pass">
</li>
<li>
	<label for="">请确认密码</label>
	<input type="password" name="repass" class="repass">
</li>
<li>
    <label for="">验证手机</label>
	<input type="text" name="mobile" class="mobile">
</li>
//获取用户名文本框的数据
var name=$('.name').val();
var pass=$('.pass').val()
var repass=$('.repass').val()
var mobile=$('.mobile').val()
```
好在JQuery的ajax方法中我们可以使用`serialize()`方法一次性的获取所有的表单数据并且把数据转化成键值对的形式,让我们方便了太多。

```JavaScript
var value=$("#ajaxForm").serialize();
console.log(value);
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_13.png)

### 解释二：接口化开发：前后端会事先约定传递的参数格式，以及返回的数据格式。后面的开发都按照着这样约定的格式。

举例：

```
{
  $arr = array(
	 'code'=>10001,
	 'msg'=>'注册成功',
	 'result'=>array('name'=>'yangying', 'age'=>18)
		);
	} else {
		$arr = array(
			'code'=>10002,
			'msg'=>'注册失败',
			'result'=>'一些信息'
		);
	}
```

采用接口化开发时，当注册成功时，会发送一些事先约定好的数据，当注册失败时，也会发送一些事先约定好的数据。

### 解释三：什么叫做接口

在该例子中的接口是`'regisiter.php'`;其实URL地址说得高级一点就是接口。

### 解释四：register.php

`if($_POST['name']) {}`

//只要name这个表单项填写了内容，即认为注册成功,

//不去复杂的判断密码的格式、用户名的格式，减轻服务端的压力

//但是如果没有填name这个表单项，则认为注册失败。

`success:function(data){console.log(data);}`

当注册成功时，服务器发送一些成功的信息，当注册失败时，服务器发送一些失败的信息。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_14.png)

解释五：

当我们没有填写用户名时，服务器就会发送失败的信息，我们都知道通过表单验证很容易就知道我们到底有没有填用户名，相当于发送了这个请求是无用的，如果我们发送过多这样的无用的请求，服务器就承受了太大的压力，所以我们必须在发送请求之前拦截一些信息，减轻服务端的压力。

```javascript
beforeSend:function(){
    if($('.name').val()==''){
		$('.tips p').text('用户名不能为空').show().delay(1500).fadeOut();
		//终止请求
		return false;
	   }
	},
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_15.png)

大家可以从图中看到即使用户名为空，也没有发送post请求，请求`register.php`，在发送之前已经被拦截了下来，这就是`beforeSend`函数的作用。

但是如果我们去掉`return false`呢，我们可以看下会有什么样的结果？

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_16.png)

大家会看到还是发送了post请求，相当于根本就没有拦截成功，所以一定要加上return false。

### 解释六：注册成功时跳转到一个页面。

```
success:function(data){
	//在接口中约定了注册成功的code为10001，注册失败的code为10002
		if(data.code!=10001){
			alert('注册失败');
			return false;
		}else{//注册成功
			location.href="https://yingy0.github.io/";
	}
},
```

### 解释七：禁止🚫重复提交

我们还需要完成一个功能，就是禁止重复提交，当完成了所有的数据验证之后，只允许提交一次就好了，就等待服务器的响应就行了，不然用户老是狂点，也就意味着不断地发起请求，这给服务器会造成很大的压力。也就是说提交按钮只提交一次后就变成禁止状态。

```javascript

var that=$(this);  //保存当前this指针的引用
if(that.is('.disabled'))
    return;
beforeSend:function(){
    if($('.name').val()==''){
		$('.tips p').text('用户名不能为空').show().delay(1500).fadeOut();
	   }
		//终止请求
		return false;
	}
that.addClass("disabled");
that.val("正在提交");
},
complete:function(){
	that.removeClass("disabled");
	that.val("注册失败请重试");
}
```

我们应该把`disabled`类写在`beforeSend`函数中，原因：假如在发送前数据验证都通过了以后，这个表单就可以提交了，提交了表单之后这个提交按钮就立马要被禁用了，换句话说，在请求的这一瞬间表单就得立马被禁用，不然用户如果狂点的话，请求就还是会被发送。

但是对于没有注册成功的用户，是需要多次可以注册的，可以多次点击的，也是需要点击多次的，所以就得释放这个禁止的功能，这个要写在`complete`函数中，因为该函数最后都得执行。

当点击提交的时候，按钮的内容编程变为正在提交，注册成功时跳到新的页面，当提交失败时按钮的内容变为提交失败请重试。

注册失败：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_17.gif)

注册成功：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1240927/o_18.gif)

现在该实现发送验证码功能了，其实获取验证码和注册其逻辑是类似的，我就不具体的说了。

怎么实现倒计时的功能，大家首先需要知道的是这个功能也应该写在`beforeSend`函数中的。

* index.html

```
//发送验证码功能
	  $('.verify').on('click',function(){
	    	var that=$(this);
	         if(that.is('.disabled')) return;
	    	var val=$('.mobile').val();
	    	$.ajax({
	    		url:'getCode.php',
	    		type:'post',
	    		dataType:'json',
	    		data:'mobile:val',
	    		beforeSend:function(){
	    			//在发送请求前需要验证
	    	       if(val==''){
	    	         $('.tips p').text('请填写手机号').show().delay(1500).fadeOut();
	    	         //终止请求
		            return false;
	              }
	              that.addClass("disabled");
	              var secondes=60;
	              var t=setInterval(function(){
	              	that.val(--secondes+'秒后再次获取');
	              	if(secondes<=0){
	              		clearInterval(t);
	              		that.removeClass("disabled");
	              	}
	              },1000)
	    		},
	    		success:function(data){},
	    		error:function(){},
	    		complete:function(){
	    		}
	    	})
	    })
```
* getCode.php

```javascript
<?php
	$arr = array(341232, 564512, 876567, 321653);
	echo $arr[array_rand($arr)];
	sleep(4);
?>
```
源代码见我的github仓库：




