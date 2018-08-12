---
title: jQuery常用方法之事件对象
comments: true
date: 2018-05-25 09:18:10
categories: 博客列表
tags: JavaScript框架
about:

---


## 事件对象

### 1. currentTarget和target

如果用JQuery方式绑定的，那么e就是JQuery的事件对象，同理：如果用DOM方式绑定的，那么e就是DOM的事件对象。

我们都知道`on()`函数给元素绑定事件有几种不同的情况，我们可以来看下。

* 第一种情况：没有父元素,直接绑定

```javascript
<body>
    <button>我是按钮</button>
</body>
<script type="text/javascript">
    $(document).ready(function(){
         $("button").on("click",function(e){
            console.log(e);
        })
  })
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_28.png)

currentTarget和target都指向子元素。

第二种情况：有父元素，给子元素绑定

```
<body>
<div>
    <button>我是按钮</button>
</div>
</body>
<script type="text/javascript">
    $(document).ready(function(){
         $("div").on("click","button",function(e){
            console.log(e);
        })
  })
</script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_30.png)

currentTarget和target都指向子元素。

第三种情况：有父元素，给父元素绑定事件。

```javascript
<body>
<div>
    <button>我是按钮</button>
</div>
</body>
<script type="text/javascript">
    $(document).ready(function(){
         $("div").on("click",function(e){
            console.log(e);
        })
  })
</script>

```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_31.png)

currentTarget指向父元素、target指向子元素。

总结：

* currentTarget的值等于this,给谁绑的事件就指向谁，也就是说永远指向的是事件绑定对象。

* target:一直指向真正触发事件的对象(事件源)，通过冒泡的方式触发了父元素的单击事件.

### 2. preventDefault()：阻止默认事件行为的触发

大家都知道提交按钮会改变浏览器的默认行为，`<a>`标签会有自动跳转到指定链接的默认行为，现在我们可以利用
`preventDefault()`事件来阻止提交按钮的默认行为，阻止`<a>`链接跳转的默认行为。

举例：

```javascript
<body>
    <input type="submit" value="我是提交按钮"></input>
    <a href="http://www.baidu.com"></a>>
</body>
<script type="text/javascript">
            $(document).ready(function() {
                $("input").on("click",function(e) {
                    e.preventDefault();
                })
                $("a").on("click",function(e) {
                    e.preventDefault();
                })
            })
        </script>
```
运行结果：`<a>`链接不跳转，提交按钮没有改变浏览器的默认行为。

### 3. stopPropagation()：阻止事件冒泡

```javascript
<!DOCTYPE html>
<html>
   <head>
        <meta charset="UTF-8">
        <title></title>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {
                $("div").on("click","button", function(e) {
                    console.log("我是按钮");
                    e.stopPropagation();
                })
                $("div").on("click", function(e) {
                     console.log("我是div");
                })
            })
        </script>
    </head>
    <body>
        <div>
            <div>
                <button>我是按钮</button>
            </div>
            <div>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_32.png)

没有阻止冒泡：删除`e.stopPropagation()`

因为给`<div>`绑定单击事件，有两层的`div`，两层的`div`下面都有`<button>`元素，所以总共是4次冒泡

如果换成这样的形式,给`<button>`绑定单击事件，就是三次冒泡。

```javascript
<body>
        <div>
            <div>
                <button>我是按钮</button>
            </div>
            <div>
    </body>
 <script type="text/javascript">
            $(document).ready(function() {
                $("button").on("click",function(e) {
                    console.log("我是按钮");
                   // e.stopPropagation();
                })
                $("div").on("click", function(e) {  
                     console.log("我是div");
                })
            })
        </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_34.png)

阻止了冒泡：添加`e.stopPropagation()`;

```javascript
<script type="text/javascript">
            $(document).ready(function() {
                $("div").on("click","button", function(e) {
                    console.log("我是按钮");
                    e.stopPropagation();
                })
                $("div").on("click", function(e) {
                     console.log("我是div");
                })
            })
        </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_33.png)

### 4.data：获取on()的第三个参数值,这个参数可以是数值型、对象性、字符串型、数组型等。

注意：这里必须用on的方式绑定事件

举例：

```javascript
<body>
   <div>
      <button>我是按钮</button>
   </div>
</body>
<script type="text/javascript">
         $(document).ready(function() {
            $("div").on("click","button",1111, function(e) {
                 console.log(e.data);
                })
          $("div").on("click","button",{"a":1111}, function(e) {
                 console.log(e.data);
                })
            })
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_35.png)

## 链式编程: return this;

在javascript中的存在的原型链：

```javscript
        <script>
            function Person(name, age) {
                this.name = name;
                this.age = age;
                this.setAge = function() { //这里的this指的是新创建出来对象p
                    this.age++;
                    console.log(this.age)
                    return this; //这里的this指的是调用的对象p
                }
                this.show=function(){
                    return this;
                }
            }
            var p = new Person("张三","20");
            p.setAge().setAge().show().setAge();
        </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_36.png)

只要函数中有`return this`，就可以组成原型链。

注意：在jQuery中，一旦出现获取操作，JQuery的链式就断掉了。因为获取属性的值一般返回的是一个字符串，而不是一个对象，没办法在加入到链中。也就是说获取属性操作是jQuery链接的终点。

在jQuery中，任意多个设置属性操作可以组成一个较长的链条

```javascript
<script>
$(document).ready(function(){
    $("div")
        .css("background-color","red") //设置值
        .css("font-size","30px")//设置值
        .css("color")//获取值
    })
</script>
```

## 隐式迭代

默认情况下，会自动迭代执行jQuery选择出来所有DOM元素的操作。

如果获取的是多元素的值，默认返回的是第一个元素的值。



