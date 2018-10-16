---
title: jQuery常用方法之css
comments: true
date: 2018-06-04 15:09:31
categories: 前端
tags: JavaScript框架
about:

---

## css->css

### 1.CSS()：获取匹配元素集合中的第一个元素的样式属性的计算值

* 设置单个样式属性:

 `$('.test').css('background-Color', 'purple');`

* 设置多个样式属性:

```javascript
$("#test").css({
  'background-color': '#ffe', 
  'border-left': '5px solid #ccc'
})
```

* 获取css样式，一次只能获取一个css样式值，如果想要获取多个值，可以利用each()方法循环遍历。

```javascript

$('.test').css('background-Color');
$('.test').css('border-left');
```

注意:

* 在css属性中，如果有`'-'`连接的属性一定要带单引号或者双引号，没有用特殊字符连接的属性可以不用带引号。总而言之，引号还是带着比较好。

* 获取速写的CSS属性(例如： `margin, background, border`) 是不支持的，虽然有些浏览器有这个功能，但是不能保证。

* 如果你想获取已经渲染的`border-width`，可以使用`$( elem ).css( "borderTopWidth" )`, 
`$( elem ).css( "borderBottomWidth" )`，其他的也是如此，依此类推。

举例：

* 1.1.点击div，得到它的背景颜色

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                width: 60px;
                height: 60px;
                margin: 5px;
                float: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <span id="result">&nbsp;</span>
        <div style="background-color:blue;"></div>
        <div style="background-color:rgb(15,99,30);"></div>

        <div style="background-color:#123456;"></div>
        <div style="background-color:#f11;"></div>
        <script>
            $(function(){
            $("div").click(function() {
                var color = $(this).css("background-color");
                $("#result").html("That div is <span style='color:" +
                    color + ";'>" + color + "</span>.");
            });
        })
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_28.png)

* 1.2.点击div，得到宽度，高度，文本颜色，背景颜色。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                height: 50px;
                margin: 5px;
                padding: 5px;
                float: left;
            }
            #box1 {
                width: 50px;
                color: yellow;
                background-color: blue;
            }
            #box2 {
                width: 80px;
                color: rgb(255, 255, 255);
                background-color: rgb(15, 99, 30);
            }
            #box3 {
                width: 40px;
                color: #fcc;
                background-color: #123456;
            }
            #box4 {
                width: 70px;
                background-color: #f11;
            }
        </style>
        <script src="http://code.jquery.com/jquery-latest.js"></script>
    </head>
    <body>
        <p id="result">&nbsp;</p>
        <div id="box1">1</div>
        <div id="box2">2</div>
        <div id="box3">3</div>
        <div id="box4">4</div>
        <script>
            $("div").click(function() {
                var html = ["The clicked div has the following styles:"];

                var styleProps = $(this).css(["width", "height", "color", "background-color"]);
                $.each(styleProps, function(prop, value) {
                    html.push(prop + ": " + value);
                });

                $("#result").html(html.join("<br>"));
            });
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_29.png)

* 1.3.当你点击一个div的时候递增他的尺寸

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                width: 20px;
                height: 15px;
                background-color: #f33;
            }
        </style>
        <script src="../jquery.min.js"></script>	
    </head>
    <body>
        <div>click</div>
        <div>click</div>
        <script>
            $(function(){
            $("div").click(function() {
                $(this).css({
                    width: function(index, value) {
                        return parseFloat(value) * 1.2;
                    },
                    height: function(index, value) {
                        return parseFloat(value) * 1.2;
                    }
                });
            });
        })
        </script>
    </body>
</html>
```

## css->位置

### 2.offset():获取/设置指定元素的左上角的位置

如果传参数就是设置`offset(top，left)`的值，如果不传参数就是获取`offset(top,left)`中的值。如果没有设置top和left的值，就获取的是原来有的值，如果设置了新的top和left的值，新的值就会覆盖原来的值，利用`offset(top，left)`获取到的自然是新的top和left的值。

```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width:200px;
            height:200px;
            background-color: #0099CC;
        }
    </style>
</head>
<body>
    <div></div>
</body>
<script src="../js/jquery.min.js" type="text/javascript"></script>
<script type="text/javascript">
    $(document).ready(function () {
        $("div").offset({
            top:10,
            left:10
        })
        var obj=$("div").offset();
        console.log("left:"+obj.left+",top:"+obj.top);
      });
</script>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_15.png)

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_16.png)

注意：

* 在设置left和top值的时候，只能传数值类型，不能传字符串。

```javascript

("div").offset({
    top:10;
    left:10;
    })
//写成下面的形式是错的
("div").offset({
    top:"10px";
    left:"10px";
    })

```

* 如果没有设置position定位，那么position的默认值为static，也就是 `position=static`，如果设置了offset()方法之后，position的值就变成了relative，也就是`position=relative`。

* 如果你自己设置了position，并且不是static，那么position的值就是你自己设置的定位，就算设置了offset()方法之后，position的值也不会变成relative，position的值依旧是你设置的那个值。

* jQuery不支持获取隐藏元素的偏移坐标。同样的，也无法取得隐藏元素的border, margin,或 padding 信息。

* 若元素的属性设置的是 visibility:hidden，那么我们依然可以取得它的坐标。但是若设置的属性是 display:none，由于在绘制 DOM 树时根本就不绘制该元素，所以它的位置属性值是 undefined。

举例：

* 2.1.点击查看位置

```javascript
<!DOCTYPE html>
<html>

    <head>
        <meta charset="utf-8" />
        <style>
            p {
                margin-left: 10px;
                color: blue;
                width: 200px;
                cursor: pointer;
            }
            span {
                color: red;
                cursor: pointer;
            }
            div.abs {
                width: 50px;
                height: 50px;
                position: absolute;
                left: 220px;
                top: 35px;
                background-color: green;
                cursor: pointer;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>

    <body>
        <div id="result">Click an element.</div>
        <p>
            This is the best way to <span>find</span> an offset.
        </p>
        <div class="abs">
        </div>
        <script>
            $(function(){
            $("*", document.body).click(function(e) {
                var offset = $(this).offset();
                e.stopPropagation();
                $("#result").text(this.tagName + " coords ( " + offset.left + ", " +
                    offset.top + " )");
            });
        })
        </script>

    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_30.png)

### 3.position：获取匹配元素中第一个元素的当前坐标,相对于最近的那个具有定位的父元素。

返回一个object，包含left和top属性

```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
        *{
            margin: 0;
            padding: 0;
        }
        p{
            position: absolute;
            top:100px;
            left: 50px;
        }
    </style>
</head>
<body>
        <p>我是p元素</p>
</body>
<script src="../js/jquery.min.js" type="text/javascript"></script>
<script type="text/javascript">
    $(document).ready(function () {
        console.log($("p").position());
      });
</script>
</html>
```

注意：因为`position()`方法不接受任何参数的值，索引`position()`方法只能获取已经存在的值，而不能设置值。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_17.png)

### 4.scrollTop()：获取或者设置窗口垂直方向的滚动的位置

浏览器和scrollTop值存在着某种比例关系，这是由浏览器内部决定的。

如果传参数就是设置`scrollTop()`的值，如果不传参数就是获取`scrollTop()`中的值。

scrollTop的值为可滚动区域在可视区域的上方被隐藏区域的高度。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_11.png)

如果没有设置`scrollTop()`的值，而是直接获取`scrollTop()`值的话，获取到的值是0，如果设置了`scrollTop()`的值，那么获取到的值就是设置的值。

如果在页面中没有滚动条的话，设置不了值，也获取不了值。都白搭，可以设置值和获取值的前提是一定要有滚动条。(右滚动条和下滚动条)

### 5.scrollLeft()：获取或者设置元素水平方向滚动的位置

如果传参数就是设置`scrollLeft()`的值，如果不传参数就是获取`scrollLeft()`中的值。

如果没有设置`scrollLeft()`的值，而是直接获取`scrollLeft()`值的话，获取到的值是0，如果设置了`scrollLeft()`的值，那么获取到的值就是设置的值。

```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
    div{
        width: 300px;
        height: 300px;
        background-color:#FF0000;
        overflow: scroll;
    }
    p{
        width: 1500px;
        height: 1500px;
    }
    </style>
    <script type="text/javascript" src="../js/jquery-1.11.1.min.js" ></script>
    <script type="text/javascript">
    $(function(){
        //设置scrollTop值
        $("input:eq(0)").click(function(){
            $("div").scrollTop("100");
        })
        //获取scrollTop值
        $("input:eq(1)").click(function(){
            console.log($("div").scrollTop());
        })
        //设置scrollLeft值
        $("input:eq(2)").click(function(){
            $("div").scrollLeft("200");
        //获取scrollLeft值
        })
        $("input:eq(3)").click(function(){
            console.log($("div").scrollLeft());
        })
    })
    </script>
</head>
<body>
<input type="button" value="设置scrollTop值" />
<input type="button" value="获取scrollTop值" />
<input type="button" value="设置scrollLeft值" />
<input type="button" value="获取scrollLeft值" />
<div><p>我是p元素</p></div>
</body>
</html>

```
 设置scrollTop的值伪100，设置scrollLeft的值为100

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_18.png)

没有设置scrollTop值，直接获取，和设置了scrollTop值，获取

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_19.png)

没有设置scrollLeft值，直接获取，和设置了scrollLeft值，获取

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_20.png)

举例：5.1.固定导航栏

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }
            .main {
                width: 865px;
                margin: 0 auto;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
           <script type="text/javascript">
        $(window).load(function() {
            var topHeight = $(".top").height();
            var navHeight = $(".nav").height();
            $(window).scroll(function () {
                var docSccrollTop = $(document).scrollTop();
                //如果scrollTop的值大于顶部图片的高度
                if(docSccrollTop > topHeight){
                    $(".nav").css({
                        "position":"fixed","top":0
                    });
                    // 此时 nav的位置固定，如果不设置 main部分的margin-top的话，将有一部分内容被挡住 nav的高度
                   $(".main").css("margin-top", navHeight);
                }else{
                   $(".nav").css({"position":"static"});  /*静态定位*/
                    $(".main").css("margin-top",0);
                }
            });
        });
    </script>
    </head>
    <body>
        <div class="top">
            <img src="imgs/top.png" />
        </div>
        <div class="nav">
            <img src="imgs/nav.png" />
        </div>
        <div class="main">
            <img src="imgs/main.png" />
        </div>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_31.png)

注意：

* 顶部头图片的高度只需要获取一次就好了，所有不用绑定在scroll()函数中，

* scroll()表示每次滚动都会触发的事件。

## css->尺寸

### 6.height()：获取匹配元素集合中的第一个元素的当前计算高度值

如果传参数就是设置`height`的值，如果不传参数就是获取`height`中的值。

### 7.width()：获取匹配元素集合中的第一个元素的当前计算宽度值

如果传参数就是设置`width`中的值，如果不传参数就是获取`width`中的值。

关于JQuery获取图片的高度和宽度时存在着一些问题。

问题总结：

* 当获取图片的高度和宽度时候，有可能能获取到，但是一刷新图片的高度和宽度就变成0了。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js" ></script>
        <script type="text/javascript">
        //此时JQuery在入口函数中获取不到图片的高度，因为jQuery的执行时机，在DOM树加载之后就开始执行，图片这时候还没有加载进来。
            $(document).ready(function(){
                var height=$("img").height();
                var width=$("img").width();
                console.log(height);
                console.log(width);
            })
        </script>
    </head>
    <body>
        <img  src="imgs/001.jpg"/>
    </body>
</html>
```

可能能获取到的原因是浏览器觉得你可能需要获取到这张图片的宽度和高度，所以浏览器就告诉你了，所以一刷新图片的高度和宽度又变成0了。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_13.png)

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_14.png)

* css()获取高度和height获取高度的区别？

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_12.png)

如何动态的设置图片的高度？

如果传参数就是设置height的值，如果不传参数就是获取height中的值。

解决办法：

* 在JavaScript的入口函数中可以混用JQuery的代码，在JQuery的入口函数中可以混用JavaScript的代码，同样可以获取到图片的高度。

```javascript
        <script type="text/javascript">
        //在javaScript的入口函数，会等到所有的外部文件加载完才开始，索引肯定能获取到图片的高度和宽度
            window.onload=function(){
                var height=$("img").height();
                var width=$("img").width();
                console.log(height);
                console.log(width);
            }
        </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_13.png)

* 不仅可以使用onload()函数，还可以使用load()函数，在javascript中，"onload()"和"load()"的功能是一样的。

```javascript

        <script type="text/javascript">
            window.load=function(){
                var height=$("img").height();
                var width=$("img").width();
                console.log(height);
                console.log(width);
            }
        </script>
```


