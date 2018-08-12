---
title: jQuery常用方法之文档处理
comments: true
date: 2018-05-14 15:09:31
categories: 博客列表
tags: JavaScript框架
about:

---

## 文档处理->内部插入

### 1.append():追加元素，往指定元素添加新创建的元素，添加到所有子元素的最后边。

* 参数可以为jQuery对象

* 参数可以为html标签

```javascript

<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            p {
                width: 100%;
                height: 50px;
                background-color: red;
            }
        </style>
        <script src="../js/jquery.min.js" type="text/javascript"></script>
    <script type="text/javascript">
        $(document).ready(function() {
            $('button').on('click', function() {
                $('div').append('<p>我是追加进去的</p>');
            });
        });
    </script>
    </head>
    <body>
        <button>我是按钮</button>
        <div>
            <span></span>
            <span></span>
            <span></span>
        </div>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_3.png)

举例：

* 1.1.在所有的段落内的尾部，追加一些HTML。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>I would like to say: </p>
        <script>
            $(function(){
            $("p").append("<strong>Hello</strong>");})
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_35.png)

* 1.2.在所有的段落内的尾部，追加一个元素。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="http://code.jquery.com/jquery-latest.js"></script>
    </head>
    <body>
        <p>I would like to say: </p>

        <script>
            $(function() {
                $("p").append(document.createTextNode("Hello"));
            })
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_34.png)

* 1.3.在所有的段落内的尾部，追加一个jQuery对象

```javascript
<!DOCTYPE html>
<html>

    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>

    <body>
        <strong>Hello world!!!</strong>
        <p>I would like to say: </p>
        <script>
            $(function() {
                $("p").append($("strong"));
            })
        </script>
    </body>

</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_33.png)

### 2.appendTo():把新创建的元素添加到指定元素，也是添加到所有子元素的最后边。

```javascript

 <script type="text/javascript">
        $(document).ready(function() {
            $('button').on('click', function() {

              $('<p>我是添加进去的</p>').appendTo("div");

            });
        });
    </script>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_3.png)

`append()`方法和`appendTo()`方法只是写的格式不同，其实现的效果是相同的。

举例：

* 2.1.将所有的span插入到ID为`"foo"`的元素内的末尾。

```javascript
<!Doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>appendTo demo</title>
        <style>
            #foo {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <span>I have nothing more to say... </span>
        <div id="foo">FOO! </div>
        <script>
            $(function() {
                $("span").appendTo("#foo");
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_32png)

## 文档处理->外部插入

### 3.`after()`：在同级的后面添加元素

```javascript
<script type="text/javascript">
        $(document).ready(function() {
            $('button').on('click', function() {

                $('span').after('<p>我是在后面加进去的</p>');
            });
        });
    </script>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_4.png)

`after()`允许我们传入一个函数，该函数返回要被插入的元素。

```javascript
$('p').after(function() {
  return '<div>' + this.className + '</div>';
});

```

实例：

* 3.1.在所有的段落后插入一些HTML

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>I would like to say: </p>
        <script>
            $(function() {
                $("p").after("<b>Hello</b>");
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_36.png)

* 3.2.在所有的段落后插入一个DOM元素

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>

    <body>
        <p>I would like to say: </p>
        <script>
            $(function(){
            $("p").after(document.createTextNode("Hello"));
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_37.png)

* 3.3.在所有段落后插入一个jQuery对象

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <b>Hello</b>
        <p>I would like to say: </p>
        <script>
            $(function(){
            $("p").after($("b"));})
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_38.png)

### 4.before()：在同级的前面添加元素

```javascript
<script type="text/javascript">
        $(document).ready(function() {
            $('button').on('click', function() {
                $('span').before('<p>我是在前面加进去的</p>');
            });
        });
    </script>
```

举例：

* 4.1.在所有的段落前插入一些HTML

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p> is what I said...</p>
        <script>
            $(function(){
            $("p").before("<b>Hello</b>");
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_39.png)

* 4.2.在所有的段落前插入一个DOM元素

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p> is what I said...</p>
        <script>
            $(function() {
                $("p").before(document.createTextNode("Hello"));
            })
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_40.png)

* 4.3.在所有段落前插入一个jQuery对象

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>

    <body>
        <b>Hello</b>
        <p> is what I said...</p><b>Hello</b>
        <script>
            $(function(){
            $("p").before($("b"));})
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_41.png)

## 文本处理->删除

### 5.empty()方法：清空指定元素中的所有后代节点

举例：

```
<!DOCTYPE html>
<html>
    <head>
        <meta  charset="utf-8"/>
        <style>
            p {
                background: yellow;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>
            Hello, <span>Person</span>
            <a href="javascript:;">and person</a>
        </p>
        <button>清空empty()</button>
        <script>
            $(function() {
                $("button").click(function() {
                    $("p").empty();
                });
            })
        </script>
    </body>
</html>
```

### 6.remove()方法：删除所有的节点，包括自己本身

当某个节点用`remove()`方法删除后，该节点所包含的所有后代节点将同时被删除。这个方法的返回值是一个指向已被删除的节点的引用，因此可以再以后继续使用这些元素。

```javascript
<ul>
<li>d苹果<li/>
<li>橘子<li/>
<li>菠萝<li/>
</ul>

<script>
var $li=$("ul li:eq(1)").remove();

$("ul").append("$li");  //把刚才删除的节点又重新添加到<ul>中
<script/>
```

举例：

* 6.1.从DOM中移除所有含有`"Hello"`的段落

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <style>
            p {
                background: yellow;
                margin: 6px 0;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p class="hello">Hello</p>
        how are
        <p>you?</p>
        <button>删除节点remove()</button>
        <script>
            $(function() {
                $("button").click(function() {
                    $("p").remove(":contains('Hello')");
                });
            })
        </script>
    </body>
</html>
```