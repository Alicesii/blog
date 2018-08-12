---
title: jQuery常用方法之事件
comments: true
date: 2018-05-22 15:09:31
categories: 博客列表
tags: JavaScript框架
about:

---

## 事件->页面载入

### 1.ready()：当DOM准备就绪时，指定一个函数来执行。

描述：

* `ready()`方法提供了一种方法，使得一旦页面的文档对象模型（DOM）变为安全的操纵，就立即运行JavaScript代码。这往往是执行与页面的用户视图或交互之前需要任务的好时机。

* 浏览器还提供了window对象上的load事件。当这个事件触发时候，表明该网页上的所有资源已加载，包括图像。此事件可以使用jQuery的`$( window ).on( "load",function(){})`监听。当代码依赖加载的资源情况下，（例如，必需知道图像的尺寸时），那么代码应放置在一个load事件的处理程序中。

* `ready()` 方法通常用于一个匿名函数：

```javascript

//第一种方式
$( document ).ready(function() {
});

//第二种方式
$(function() {

});
```

## 事件->事件处理

### 2.bind()：为一个元素绑定一个事件处理程序

3.0以上的版本已经废除了

```javascript
<script>
$(document).ready(function(){
    //绑定一个事件
    $("#btn").bind("click",function(){
    })
    //绑定多个事件
    $("#btn").bind("click  mouseenter",function(){
    })
})
</script>
```

可以通过传递一个事件类型/处理函数的数据键值对映射来绑定多个事件处理程序

```javascript
$('#foo').bind({
  click: function() {

  },
  mouseenter: function() {

  }
});
```


举例：

* 2.1.为段落标签绑定单击和双击事件。

注意：坐标是相对于窗口的，在这个示例中是相对于`iframe`的。

```javascript
<!DOCTYPE html>
<html>
<head>
        <style>
            p {
                background: yellow;
                font-weight: bold;
                cursor: pointer;
                padding: 5px;
            }
            p.over {
                background: #ccc;
            }
            span {
                color: red;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>Click or double click here.</p>
        <span></span>
        <script>
            $(function() {
                $("p").bind("click", function(event) {
                    var str = "( " + event.pageX + ", " + event.pageY + " )";
                    $("span").text("Click happened! " + str);
                });
                $("p").bind("dblclick", function() {
                    $("span").text("Double-click happened in " + this.nodeName);
                });
                $("p").bind("mouseenter mouseleave", function(event) {
                    $(this).toggleClass("over");
                });
            })
        </script>
    </body>
</html>
```

* 2.2.点击段落时，显示其中的内容：

```javascript
$("p").bind("click", function(){
alert( $(this).text() );
});
```

* 2.3.在事件处理之前，可以传入一些额外的数据：

```javascript
function handler(event) {
alert(event.data.foo);
}
$("p").bind("click", {foo: "bar"}, handler)

```

* 2.4.通过返回false的方式取消默认的动作，并防止它进行事件冒泡：

```javascript
$("form").bind("submit", function() { return false; })
```

* 2.5.通过使用`.preventDefault()`方法，仅取消默认的动作。

```javascript
$("form").bind("submit", function(event) {
event.preventDefault();
});
```

* 2.6.通过使用`.stopPropagation()`方法，防止事件冒泡，但是默认执行默认的动作。

```javascript
$("form").bind("submit", function(event) {
  event.stopPropagation();
});

```

* 2.7.同时绑定多个事件。

```javascript
$("div.test").bind({
  click: function(){
    $(this).addClass("active");
  },
  mouseenter: function(){
    $(this).addClass("inside");
  },
  mouseleave: function(){
    $(this).removeClass("inside");
  }
});
```

### 3.unbind()：从元素上删除一个以前附加事件处理程序

```javascript
<script>
$(document).ready(function(){
        //解绑一个事件
    $("#btn").bind("click",function(){
      //解绑了选择到元素的click事件s
       $("#btn").unbind("click");
    })
        //解绑多个事件
    $("#btn").bind("click mouseenter",function(){
          //解绑了选择到元素的click事件
       $("#btn").unbind("click");
       $("#btn").unbind("mouseenter");
     })
})
</script>
```

上面的代码绑定了几个事件就要写几次绑定比较的麻烦，如果绑定的时候给每个事件加上`".namespace"`，那么解绑的时候，只需要写一次就好了。

```javascript

$(document).ready(function(){
    //绑定一个事件
    $("#btn").bind("click.namespace",function(){
    })
    //绑定多个事件
    $("#btn").bind("mouseenter.namespace",function(){
    })
     $("#btn1").bind("click",function(){
        $("#btn1").unbind(".namespace");
    })
})
```

注意：`bind()`、`click()`事件都无法为动态创建的元素绑定事件===>解决办法：用`on()`方法来绑定事件

### 4.on()：在选定的元素上绑定一个或多个事件处理函数

和delegate方法类似，如果需要为子元素绑定事件，直接给父元素绑定即可，并且支持动态创建的元素的绑定

语法格式：`$(selector).on( events [, selector ] [, data ], handler )`

参数介绍：

* 第一个参数：`events`，事件名

* 第二个参数：`selector`,类似`delegate`(子选择器)

* 第三个参数: 传递给事件响应方法的参数

* 第四个参数：`handler`，事件处理函数

举例：

```javascript
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function(){
                //动态给p元素添加子元素
                $("input:eq(2)").click(function(){
                    $("p").append("<h1>我是动态创建的元素</h1>")
                })
                //使用on的方式给p元素的所有子元素绑定事件
                $("p").on('click','input',function(){
                    console.log($(this).val());
                })
            })
        </script>
    </head>
    <body>
       <p>
        <input type="button" value="绑定事件"/>
        <input type="button" value="解绑事件"/>
        <input type="button" value="动态创建元素"/>
       </p>
       <div></div>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_21.png)

`on()`不仅可以给指定元素的子元素绑定事件，还可以给自身绑定事件,很简单，就不需要指定第二个参数即可。

```javascript
//使用on的方式给p元素本身添加事件
 $("p").on('click',function(){

 })
```

注意：如果省略selector或者是null，那么事件处理程序被称为直接事件或者直接绑定事件。每次选中的元素触发事件时，就会执行处理程序，不管它直接绑定在元素上，还是从后代（内部）元素冒泡到该元素的

说明：

* 当提供selector参数时，事件处理程序是指为委派事件。事件不会在直接绑定的元素上触发，但当selector参数选择器匹配到后代（内部元素）的时候，事件处理函数才会被触发。jQuery会从`event target `开始向上层元素(例如，由最内层元素到最外层元素)开始冒泡，并且在传播路径上所有绑定了相同事件的元素若满足匹配的选择器，那么这些元素上的事件也会被触发。

* 委托事件有一个极大的优势，他们能在后代元素添加到文档后，可以处理这些事件。在确保所选择的元素已经存在的情况下，进行事件绑定时，您可以使用委派的事件，以避免频繁的绑定事件及解除绑定事件。

例如，在一个表格的tbody中含有1,000行，下面这个示例会为这1,000元素绑定事件：

```javascript
$( "#dataTable tbody tr" ).on( "click", function() {
  console.log( $( this ).text() );
});
```

一个委派事件的方法只在一个元素上绑定一个事件处理程序，下面的代码是绑定在tbody元素上，并且事件只会向上冒泡一层（从被点击的tr 到 tbody ）:

```javascript
$( "#dataTable tbody" ).on( "click", "tr", function() {
  console.log( $( this ).text() );
});

```

举例：

```javascript
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style type="text/css">
            p{
                width: 300px;
                height: 100px;
                background-color: aqua;
            }
        </style>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function(){
                //动态给p元素添加子元素
                $("input:eq(2)").click(function(){
                    $("p").append("<h1>我是动态创建的元素</h1>")
                })
                //使用on的方式绑定事件
                $("p").on('click','input',function(){
                    console.log($(this).val())
                })
                $("p").on('click',function(){
                    console.log($(this).html())
                })
            })
        </script>
    </head>
    <body>
       <p>
        <input type="button" value="绑定事件"/>
        <input type="button" value="解绑事件"/>
        <input type="button" value="动态创建元素"/>
       </p>
       <div></div>
    </body>

</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_22.png)

调用`event.stopPropagation() `和 `event.preventDefault()`会从一个事件处理程序会自动返回false。也可以直接将false当作handler的参数，作为 `function(){ return false; } `的简写形式

实例：

* 4.1.当点击段落时，显示该段落中的文本：

```javascript
$("p").on("click", function(){
alert( $(this).text() );
});
```

* 4.2.向事件处理函数中传入数据，并且在事件处理函数中通过名字来获取传入的数据：

```javascript
function myHandler(event) {
  alert(event.data.foo);
}
$("p").on("click", {foo: "bar"}, myHandler)
```

* 4.3.取消表单的提交动作，并且通过返回 false 的方法来防止事件冒泡：

```javascript
$("form").on("submit", false)
```

* 4.4.通过使用`preventDefault()`，仅取消默认的动作。

```javascript
$("form").on("submit", function(event) {
  event.preventDefault();
});
```

* 4.5.通过使用`.stopPropagation()`，防止提交事件的冒泡行为，但是并不禁止提交行为。

```javascript
$("form").on("submit", function(event) {
  event.stopPropagation();
});
```

* 4.6.传递一个data数据给`.trigger()`的事件处理程序，作为第二个参数。

```javascript
$( "div" ).on( "click", function( event, person ) {
  alert( "Hello, " + person.name );
});
$( "div" ).trigger( "click", { name: "Jim" } );

```

* 4.7.传递一个数组给`.trigger()`的事件处理程序，作为第二个参数。

```javascript
$( "div" ).on( "click", function( event, salutation, name ) {
  alert( salutation + ", " + name );
});
$( "div" ).trigger( "click", [ "Goodbye", "Jim" ] );
```


### 5.off()：解绑事件

关于事件的解绑，有两种比较重要的方式，大家一定要知道的。

* 第一种形式: `$("p").off()`

举例：

```javascript
<body>
       <p>
        <input type="button" value="绑定事件"/>
        <input type="button" value="解绑事件"/>
        <input type="button" value="动态创建元素"/>
        <input type="button" value="on事件的解绑"/>
       </p>
       <div></div>
    </body>
     <script type="text/javascript">
            $(document).ready(function(){
                //动态给p元素添加子元素
                $("input:eq(2)").click(function(){
                    $("p").append("<h1>我是动态创建的元素</h1>")
                })
                //使用on的方式绑定事件
                $("p").on('click','input',function(){
                    console.log($(this).val())
                })
                $("p").on('click',function(){
                    console.log($(this).html())
                })
                $("input:eq(3)").on('click',function(){
                    $("p").off('click');
                })
            })
        </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_23.png)

注意：利用`$('p').off('click')`;的方式移除了所有`on()`触发的事件，因为动态创建这个元素的click事件并不是由`on()`事件触发的，而是由`click()`事件触发的，所以利用`off()`的方式解绑不了。

第二种形式:`$( "p").off( "click", "**" );`

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_24.png)

从图中可以看出,通过这样的方式,解除了除给自身绑定事件之外的其他事件.,例如通过父元素给子元素绑定事件，父元素的绑定的事件没有解除，但是子元素绑定的事件解除了，也就是取消掉了冒泡。

### 6.one()：为元素的事件添加处理函数。处理函数在每个元素上每种事件类型最多执行一次，处理程序在第一次触发事件后会被立即解除绑定。

最常用的场景：一般提交表单的时候只允许提交一次，并不允许多次提交。

语法：`one( events [, data ], handler() )`

参数说明：

* 第一个参数：`events`，事件名

* 第二个参数：`selector`，选择器

* 第三个参数：`handler`，事件处理方法

举例：

```javascript
 <script type="text/javascript">
        $(function(){
            $("button").one('click',function(){
                console.log("只能点击一次呦");
            })
        })
    </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_25.png)

哈哈，只能触发一次，你点在多次也没有用。

那点击一次和点击多次在浏览器中有什么区别呢？

```
 <script type="text/javascript">
        $(function(){
            $("button").on('click',function(){
                console.log("可以点击多次");
            })
        })
    </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_26.png)

对比上面两张大家肯定可以区别点击一次和点击多次的区别了

`on()`不能实现`one()`的功能，只能模拟。

```javascript

$( "#foo" ).one( "click", function() {
  alert( "This will be displayed only once." );
});
//在代码执行后，点击id为foo的元素将显示警报。之后再在该元素上点击时，就不会再触发该事件。此代码是等效于：

$( "#foo" ).on( "click", function( event ) {
  alert( "This will be displayed only once." );
  $( this ).off( event );
});
```

### 7.trigger()：根据绑定到匹配元素的给定的事件类型执行所有的处理程序和行为。

既触发事件，又触发浏览器的默认行为。

```javascript
body>
    <button>我是按钮</button>
<form>
<input type="submit" value="提交" />
</form>
</body>
<script type="text/javascript">
        $(function(){
            $("button").click(function(){
                $("input").trigger("click");
            })
        })
    </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_27.png)

提交按钮会改变浏览器的默认操作，如上图，给访问地址加上`"?"`问号,如果给指定元素绑定了trigger事件，那么这个元素也会改变浏览器的默认操作。

举例：

* 7.1.点击`button#2`时，同时触发`button#1`的点击事件。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            button {
                margin: 10px;
            }
            div {
                color: blue;
                font-weight: bold;
            }
            span {
                color: red;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button>Button #1</button>
        <button>Button #2</button>
        <div><span>0</span> button #1 clicks.</div>
        <div><span>0</span> button #2 clicks.</div>
        <script>
            $(function(){
            $("button:first").click(function() {
                update($("span:first"));
            });
            $("button:last").click(function() {
                $("button:first").trigger('click');
                 update($("span:last"));
            });
            function update(j) {
                var n = parseInt(j.text(), 10);
                j.text(n + 1);
            }
        })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_62.png)

* 7.2.若要提交第一个表单但又不想使用 `submit() `函数

```javascript
var event = jQuery.Event("submit");
$("form:first").trigger(event);
if ( event.isDefaultPrevented() ) {
}
```
* 7.3.向事件中传入任意的数据：

```javascript
$("p").click( function (event, a, b) {
} ).trigger("click", ["foo", "bar"]);
```
* 7.4.通过event对象，向事件中传入任意的数据：(两种方式)

```javascript

//第一种方式
var event = jQuery.Event("logged");
event.user = "foo";
event.pass = "bar";
$("body").trigger(event);

//第二种方式
$("body").trigger({
type:"logged",
user:"foo",
pass:"bar"
});
```
### 8.triggerHandler()：为一个事件执行附加到元素的所有处理程序

描述：

* `trigger()`会影响所有与jQuery对象相匹配的元素，而`triggerHandler()`仅影响第一个匹配到的元素。

* 使用`triggerHandler()`触发的事件，并不会在DOM树中向上冒泡，如果它们不是由目标元素直接触发的，那么它就不会进行任何处理。

* 只触发事件，不会触发浏览器的默认行为。

举例：

* 8.1.如果使用`triggerHandler()`触发focus事件，那么它只会触发绑定了该事件的处理函数，而浏览器的默认focus动作是不会被触发的。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button id="old">.trigger("focus")</button>
        <button id="new">.triggerHandler("focus")</button><br/><br/>
        <input type="text" value="To Be Focused" />
        <script>
            $(function() {
                $("#old").click(function() {
                    $("input").trigger("focus");
                });
                $("#new").click(function() {
                    $("input").triggerHandler("focus");
                });
                $("input").focus(function() {
                    $("<span>Focused!</span>").appendTo("body").fadeOut(1000);
                });
            })
        </script>
    </body>
</html>
```

## 事件委派

### 9.delegate()：为所有匹配选择器（selector参数）的元素绑定一个或多个事件处理函数，基于一个指定的根元素的子集，匹配的元素包括那些目前已经匹配到的元素，也包括那些今后可能匹配到的元素。简单的说，如果需要为子元素绑定事件，直接给父元素绑定即可。

语法格式：`$(selector).delegate( selector, eventType, handler )`

参数：

* 第一个参数: `selector`， 子选择器。

* 第二个参数：事件类型

* 第三个参数：事件响应方法

```javascript
$(".parentBox").delegate("p", "click", function(){
        //为 .parentBox下面的所有的p标签绑定事件
    });
```
描述：

* 可支持动态创建元素的绑定

举例：

```javascript
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function(){
                //动态给p元素添加子元素
                $("input:eq(2)").click(function(){
                    $("p").append("<h1>我是动态创建的元素</h1>")
                })
                //使用on的方式绑定事件
                $("p").delegate('input','click',function(){
                    console.log($(this).val());
                })
            })
        </script>
    </head>
    <body>
       <p>
        <input type="button" value="绑定事件"/>
        <input type="button" value="解绑事件"/>
        <input type="button" value="动态创建元素"/>
       </p>
       <div></div>
    </body>

</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_21.png)

`delegate()`和`on()`绑定的效果是相同的，都支持绑定动态创建的元素，但是`delegate()`已经废除了

## 事件->事件切换

### 10.hover()：将两个事件函数绑定到匹配元素上，分别当鼠标指针进入和离开元素时被执行。

可以有一个参数,也可以有两个参数,参数也可以是回调函数。

`hover(mouseenter,mouseleave)`：鼠标移入，移出。

动态下拉菜单的另一种实现

```javascript
 <script type="text/javascript">
  $(document).ready(function(){
                 $(".wrap > ul > li").hover(function(){
                     $(this).children("ul").slideDown();
                 },function(){
                     $(this).children("ul").slideUp();
                 })
            })
</script>
```
又一种实现

```javascript
 <script type="text/javascript">
  $(document).ready(function(){
                $(".wrap > ul > li").hover(function(){
                     $(this).children("ul").slideToggle();
                })
            })
</script>
```

举例：

* 10.1.当鼠标在表格单元格中来回滑动的时候添加特殊的样式

```javascript
$("td").hover(
  function () {
    $(this).addClass("hover");
  },
  function () {
    $(this).removeClass("hover");
  }
);
```

## 事件->事件

### 11.scroll():事件触发或者绑定滚动事件

当用户在元素内执行了滚动操作，就会在这个元素上触发scroll事件，它适用于window对象。

```javascript
 $(selector).scroll(function(){
     //当选择的元素发生滚动的时候触发
    });
```
举例：

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                color: blue;
            }
            p {
                color: green;
            }
            span {
                color: red;
                display: none;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <div>Try scrolling the iframe.</div>
        <p>Paragraph - <span>Scroll happened!</span></p>
        <script>
            $(function() {
                $("p").clone().appendTo(document.body);
                $("p").clone().appendTo(document.body);
                $("p").clone().appendTo(document.body);
                $(window).scroll(function() {
                    $("span").css("display", "inline").fadeOut("slow");
                });
            })
        </script>
    </body>
</html>
```