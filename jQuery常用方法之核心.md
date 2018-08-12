---
title: jQuery常用方法之核心
comments: true
date: 2018-05-12 09:18:10
categories: 博客列表
tags: JavaScript框架
about:

---

## 核心->jQuery对象访问：

### 1.each():遍历一个jQuery对象，为每个匹配元素执行一个函数。

```javascript
//全局对象的形式
array=[];
$.each(array,function(index,object){})

//实例对象的形式
$("li").each(function(index,element){})
```
这两种形式的参数的顺序是相同的

描述：

* `.each()`：它会迭代jQuery对象中的每一个DOM元素。每次回调函数执行时，会传递当前循环次数作为参数(从0开始计数)。函数是在当前DOM元素为上下文的语境中触发的，因此关键字this总是指向这个元素。

* `each()`方法没有返回值,与`each()`方法功能相似的`map()`方法的返回值是一个数组。

* jQuery的方法中有一个很大的特点就是隐式迭代，所以很多情况下都用不到`each()`方法来显式的迭代，只有在某些特殊的情况下，才会需要来显式的迭代。

```
 $( "li" ).each(function() {
   $(this).addClass( "foo" );
 });
  $( "li" ).addClass( "bar" );
```
举例：

* 1.1.循环遍历给所有的`<li>`元素设置索引号，

```javascript
  <body>
        <ul>
        <li>我是li</li>
        <li>我是li</li>
        <li>我是li</li>
        <li>我是li</li>
        <li>我是li</li>
        <li>我是li</li>
        <li>我是li</li>
        </ul>
    </body>
        <script>
            $(function(){
              $("li").each(function(index,element){
                console.log(index);
                $(element).html("我是li"+index);
              })
            })
        </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_40.png)

* 1.2.遍历三个`div`并设置它们的`color`属性

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            div {
                color: red;
            }
        </style>
        <script type="text/javascript" src="../jquery.min.js"></script>
        <script type="text/javascript">
            $(function(){
            $("button").click(function() {
                    $("div").each(function(index) {
                        if(this.style.color != "blue") {
                            this.style.color = "blue";
                        } else {
                            this.style.color = "";
                        }
                    });
            });
        })
        </script>
    </head>
    <body>
        <div>我是div</div>
        <div>我是div</div>
        <div>我是div</div>
        <button>我是按钮</button>
    </body>

</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_1.png)

*  1.3.如果你不想要普通的DOM元素，而想获得的是jQuery对象的话，使用`$(this)`函数。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <style>
            ul {
                font-size: 18px;
                margin: 0;
            }          
            span {
                color: blue;
                text-decoration: underline;
                cursor: pointer;
            }
            
            .example {
                font-style: italic;
            }
        </style>
        <script src="../../js/jquery-1.11.1.min.js"></script>
    </head>
    <body>
        To do list: <span>(click here to change)</span>
        <ul>
            <li>写作业</li>
            <li>睡觉</li>
            <li>吃饭</li>
        </ul>
        <script>
            $(function() {
                $("span").click(function() {
                    $("li").each(function() {
                        console.log($(this).toggleClass("example"));
                    });
                });
            });
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_3.png)

* 1.4.如果想要提前结束`each()`循环,可以使用`return false`.

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                width: 40px;
                height: 40px;
                margin: 5px;
                float: left;
                border: 2px blue solid;
                text-align: center;
            }
            span {
                color: red;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button>我是按钮</button>
        <span></span>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div id="stop">Stop</div>
        <div></div>
        <div></div>
        <div></div>
        <script>
          $(function(){
            $("button").click(function() {
                $("div").each(function(index, document) {
                    $(document).css("backgroundColor", "yellow");
                    if($(this).is("#stop")) {
                        $("span").text("显式循环在此停止" + index);
                        return false;
                    }
                });
            });
        })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_2.png)

### 2.index()方法：从匹配的元素中搜索指定元素的索引值，从0开始计数。

返回值：

* 如果不传递任何参数给`index()`方法，则返回值就是jQuery对象中第一个元素相对于它同辈元素的位置。

* 如果在一组元素上调用`index()`,并且参数是一个DOM元素或jQuery对象.`index()`返回值就是传入的元素相对于原先集合的位置。

* 如果参数是一个选择器，index返回值就是原先元素相对于选择器匹配元素的位置。如果找不到匹配的元素，则`.index()`返回-1.

举例：

* 2.1.点击后，返回那个div在页面上的索引值(从0开始计数)。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <style>
            div {
                background: yellow;
                margin: 5px;
            }
            span {
                color: red;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <span>点我</span>
        <div>我是div1</div>
        <div>我是div2</div>
        <div>我是div3</div>
        <script>
         $(function(){
            $("div").click(function() {
                var index = $("div").index(this);
                $("span").text("点击的DIV的索引是：" + index);
            });
        });
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_5.png)

* 2.2.返回某个指定ID的元素的索引位置

```javascript
<!DOCTYPE html>
<html>
    <head>
    <meta charset="utf-8" />
        <style>
            div {
                font-weight: bold;
                color: #090;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <ul>
            <li id="li1">我是第一个li</li>
            <li id="li2">我是第二个li</li>
            <li id="li3">我是第三个li</li>
        </ul>
        <div></div>
        <script>
          $(function(){
            var listItem = $('#li3');
            $('div').html('Index: ' + $('li').index(listItem)); 
        });
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_6.png)

* 2.3.返回jQuery集合中第一项的索引值。

```javascript
<!DOCTYPE html>
<html>
    <head>
     <meta charset="utf-8" />
        <style>
            div {
                font-weight: bold;
                color: #090;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <ul>
            <li id="li1">我是第一个li</li>
            <li id="li2">我是第二个li</li>
            <li id="li3">我是第三个li</li>
        </ul>
        <div></div>
         <script>
          $(function(){
            var listItems = $('li:gt(0)');
            $('div').html( 'Index: ' + $('li').index(listItems) );
          })
         </script>
    </body>
</html>
```
* 2.4.返回某个指定ID的元素相对于所有` <li> `元素的索引值。

```javascript
<!DOCTYPE html>
<html>
    <head>
     <meta charset="utf-8" />
        <style>
            div {
                font-weight: bold;
                color: #090;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <ul>
            <li id="li1">我是第一个li</li>
            <li id="li2">我是第二个li</li>
            <li id="li3">我是第三个li</li>
        </ul>
        <div></div>
        <script>
      $(function(){
         $('div').html('Index: ' +  $('li2').index('li') );
      })
        </script>
    </body>
</html>
```

## 核心->数据缓存

### 3.data(): 数据缓存

描述：

* 获取`data`值:

```javascript
   <body>
     <button>我是按钮</button>
     //必须有"data-"前缀，这里的值可以是任何类型的值,获取值的时候不是可以获取所有类型的值，分情况。
     <div data-index= "{a:11}"|"string"|1234></div>
   </body>
    <script type="text/javascript">
        $(function(){
            $("button").click(function(){
                //没有"data-"前缀，直接写名称就好,获取值的时候分为简单数据类型和复杂数据类型。
                var data=$("div").data("index");
                console.log(data);
            })
        })
    </script>
```
* 如果是简单的类型获取的是它本身的类型的值，如果是复杂的对象获取的是字符串类型的值。

```javascript
<div data-role="page" data-last-value="43" data-hidden="true" data-options='{"name":"John"}'></div>

//获取值
$("div").data("role") === "page";
$("div").data("lastValue") === 43;
$("div").data("hidden") === true;
$("div").data("options").name === "John";
```

* 设置`data`值：不管设置的是什么类型的值，获取到的就是相对应类型的值，

```javascript
<body>
<button>我是按钮</button>
<div></div>
</body>
 <script type="text/javascript">
        $(function(){
            $("button").click(function(){
            $("div").data("name","张三"|{"xiaoming","18"}|"deiji");
            var name=$("div").data("name");
                console.log(name);
            })
        })
    </script>
```

* 当页面先进来的时候，JQuery对象本身会收集你所有的`"data-" `属性，然后放在JQuery内部维护的对象，相当于放到内存中去了，下次去操作的时候，直接操作内存，比操作DOM的效率更快。

举例：

* 3.1.当鼠标放在li上面的时候，获取一下`data-isLoad`这个属性。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <script type="text/javascript" src="../jquery.min.js" ></script>
        <script>
            $(function(){
                $("li").mouseenter(function(){
                    var data=$(this).data("isload");
                    if(data===0){
                        $(this).append("<p>我是动态获取的P元素</P>");
                        $(this).data("isload","1");
                //如果不设置"isload"的值等于"1"的话，mouseenter的事件就会一直触发，当执行一次了之后，"isload"的值等于1，不满足条件，事件就不会触发了。
                    }
                })
            })
        </script>
    </head>
     <body>
        <ul>
        <li data-isLoad = "0">菜单一</li>
        <li data-isLoad = "0">菜单二</li>
        <li data-isLoad = "0">菜单三</li>
        </ul>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_42.png)

## 核心：多库共存

大家应该都知道jQuery占用了`$ `和`jQuery`这两个变量,`Jquery`也提供了释放这两个变量的机制,释放了之后大家就可以把`$ `和`jQuery`当做普通变量来使用了。

### 4.noConflict()：释放"$"变量,也就是说放弃jQuery控制$变量。

语法格式：`jQuery.noConflict( [removeAll ] ) `

参数：`removeAll`：一个布尔值，判断是否从全局作用域中内去除所有`jQuery`变量(包括`jQuery`本身).

形式:

```javascript
var $ = { name : “itecast” };
$.noConflict();//释放"$"变量，但是此时可以使用jQuery（如果jQuery没有被占用）
var my_jQ = $.noConflict();//让jQuery释放$,让$回归到jQuery之前的对象定义上去。此时,可以使用my_jQ来代替$,可以达到一样的效果。
```
举例：

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <script>
            var $="{ name : 'itecast' }";
        </script>
        <script type="text/javascript" src="../js/jquery.min.js" ></script>
        <script>
            $(function(){
                var my_jQ = $.noConflict();
                //现在已经释放了$,可以使用my_jQ来代替$,可以实现一样的效果
                my_jQ("button").click(function(){
                    console.log("我是自定义的内容");
                })
             console.log($);
            })
        </script>
    </head>
    <body>
        <button>我是按钮</button>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_41.png)

描述：

* 如果我们需要同时使用jQuery和其他javascript库，我们可以使用` $.noConflict()`把` $`的控制权交给其他库。旧引用的`$ `被保存在jQuery的初始化;

* 如果由于某种原因，加载两个版本的jQuery（这是不推荐）， 第二个版本中调用`$.noConflict(true)`将返回全局的jQuery变量给第一个版本。

```javascript
<script type="text/javascript" src="other_lib.js"></script>
 <script type="text/javascript" src="jquery.js"></script>
 <script type="text/javascript">
     $.noConflict();
 </script>
```
* `ready()`方法可以给jQuery对象取个别名,这样就能够在传给`.ready()`的回调函数的内部继续使用`$`而不用担心冲突

```javascript
 <script type="text/javascript" src="other_lib.js"></script>
 <script type="text/javascript" src="jquery.js"></script>
 <script type="text/javascript">
     $.noConflict();
        jQuery(document).ready(function($) {
           //$符号在内部还是可以继续使用的
  });
        //$符号在ready()函数外部不能使用了，Jquery对$控制权已经交给别人了。
  </script>
```
原因：`ready()`是一个闭包，`ready()`函数的作用域是私有类型的作用域
