---
title: jQuery常用方法之筛选
comments: true
date: 2018-05-09 08:18:10
categories: 博客列表
tags: JavaScript框架
about:

---

##  筛选->查找

### 1.find():，得到当前匹配的元素集合中每个元素的后代，也就是说，既要查找子元素，还要查找其后代元素。

举例：

```javascript
<!DOCTYPE html>
<html>

  <head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../jquery.min.js"></script>
    <script>
      $(function() {
        $('ul>.item-ii').find('li').css('background-color', 'red');
      })
    </script>
  </head>

  <body>
    <ul class="level-1">
      <li class="item-i">I</li>
      <li class="item-ii">II
        <ul class="level-2">
          <li class="item-a">A</li>
          <li class="item-b">B
            <ul class="level-3">
              <li class="item-1">1</li>
              <li class="item-2">2</li>
              <li class="item-3">3</li>
            </ul>
          </li>
          <li class="item-c">C</li>
        </ul>
      </li>
      <li class="item-iii">III</li>
    </ul>

  </body>

</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_7.png)

实例：

* 1.1.为每个单词添加`span`标签，并为`span`标签添加`hover`事件，并且将含有t的单词变为斜体。

```javascript
<!DOCTYPE html>
<html>

  <head>
    <style>
      p {
        font-size: 20px;
        width: 200px;
        cursor: default;
        color: blue;
        font-weight: bold;
        margin: 0 10px;
      }
      .hilite {
        background: yellow;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>

  <body>
    <p>
      When the day is short find that which matters to you or stop believing
    </p>
    <script>
      $(function() {
        var newText = $("p").text().split(" ").join("</span> <span>");
        newText = "<span>" + newText + "</span>";

        $("p").html(newText)
          .find('span')
          .hover(function() {
              $(this).addClass("hilite");
            },
            function() {
              $(this).removeClass("hilite");
            })
          .end()
          .find(":contains('t')")
          .css({
            "font-style": "italic",
            "font-weight": "bolder"
          });
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_8.png)

### 2.children():获得匹配元素集合中每个元素的子元素，也就是说只选择直接子元素

举例：

```javascript

    <script>
      $(function() {
         $('.level-2').children().css('background-color', 'red');
      })
    </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_9.png)

实例：

* 2.1.查找被点击的元素的所有子元素。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        font-size: 16px;
        font-weight: bolder;
      }
      div {
        width: 130px;
        height: 82px;
        margin: 10px;
        float: left;
        border: 1px solid blue;
        padding: 4px;
      }
      #container {
        width: auto;
        height: 105px;
        margin: 0;
        float: none;
        border: none;
      }
      .hilite {
        border-color: red;
      }
      
      #results {
        display: block;
        color: red;
      }
      
      p {
        margin: 10px;
        border: 1px solid transparent;
      }
      
      span {
        color: blue;
        border: 1px solid transparent;
      }
      
      input {
        width: 100px;
      }
      
      em {
        border: 1px solid transparent;
      }
      
      a {
        border: 1px solid transparent;
      }
      
      b {
        border: 1px solid transparent;
      }
      
      button {
        border: 1px solid transparent;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>

  <body>
    <div id="container">

      <div>
        <p>This <span>is the <em>way</em> we</span> write <em>the</em> demo,</p>

      </div>
      <div>
        <a href="#"><b>w</b>rit<b>e</b></a> the <span>demo,</span> <button>write
       the</button> demo,
      </div>

      <div>
        This <span>the way we <em>write</em> the <em>demo</em> so</span>

        <input type="text" value="early" /> in
      </div>
      <p>
        <span>t</span>he <span>m</span>orning.
        <span id="results">Found <span>0</span> children in <span>TAG</span>.</span>
      </p>
    </div>
    <script>
      $(function() {
      $("#container").click(function(e) {
      $("*").removeClass("hilite");
      var $kids = $(e.target).children();
      var len = $kids.addClass("hilite").length;
      $("#results span:first").text(len);
      $("#results span:last").text(e.target.tagName);
      e.preventDefault();
      return false;
      });
    });
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_9.png)

### 3.parent():取得匹配元素集合中，直接的父元素，子元素会继承父元素的属性。

举例：

```javascript
    <script>
      $(function() {
         $('.item-a').parent().css('background-color', 'red');
      })
    </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_7.png)

实例：

* 3.1.显示页面中每个元素的父元素,可以查看 raw html 的源代码。

```javascript
<!DOCTYPE html>
<html>

  <head>
    <style>
      div,
      p {
        margin: 10px;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>

  <body>
    <div>div,
      <span>span, </span>
      <b>b </b>

    </div>
    <p>p,
      <span>span,
     <em>em </em>
     </span>
    </p>

    <div>div,
      <strong>strong,
      <span>span, </span>
       <em>em,
         <b>b, </b>
      </em>
     </strong>
      <b>b </b>
    </div>
    <script>
      $(function(){
      $("*", document.body).each(function() {
        var parentTag = $(this).parent().get(0).tagName;
        $(this).prepend(document.createTextNode(parentTag + " > "));
      });
    })
    </script>

  </body>
</html>

```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_10.png)

### 4.parents():获得指定祖先元素，也就是说不仅是父元素，还有父元素的祖先元素。

举例：

```javascript
<script>
      $('.item-a').parents().css('background-color', 'red');
      })
    </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_11.png)

子元素会继承父元素的属性

实例：

* 4.1.查找每个b标签的所有父元素

```javascript
<!DOCTYPE html>
<html>

  <head>
    <style>
      b,
      span,
      p,
      html body {
        padding: .5em;
        border: 1px solid;
      }
      
      b {
        color: blue;
      }
      
      strong {
        color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>

  <body>
    <div>
      <p>
        <span>
        <b>My parents are: </b>
      </span>
      </p>
    </div>
    <script>
      $(function() {
        var parentEls = $("b").parents()
          .map(function() {
            return this.tagName;
          })
          .get().join(", ");
        $("b").append("<strong>" + parentEls + "</strong>");
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_12.png)

* 4.2.点击元素，查找每个span标签的所有独一无二的div父元素

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      p,
      div,
      span {
        margin: 2px;
        padding: 1px;
      }
      div {
        border: 2px white solid;
      }
      span {
        cursor: pointer;
        font-size: 12px;
      }
      .selected {
        color: blue;
      }
      b {
        color: red;
        display: block;
        font-size: 14px;
      }
    </style>
    <script src="../../js/jquery-1.11.1.min.js"></script>
  </head>
  <body>
    <p>
      <div>
        <div><span>Hello</span></div>
        <span>Hello Again</span>

      </div>
      <div>
        <span>And Hello Again</span>
      </div>
    </p>

    <b>Click Hellos to toggle their parents.</b>
    <script>
      $(function(){
      function showParents() {
        $("div").css("border-color", "white");
        var len = $("span.selected")
          .parents("div")
          .css("border", "2px red solid")
          .length;
        $("b").text("Unique div parents: " + len);
      }
      $("span").click(function() {
        $(this).toggleClass("selected");
        showParents();
      });
    })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_13.png)

### 5.siblings()：获得匹配元素集合中每个元素的兄弟元素

举例：

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../jquery.min.js"></script>
    <script type="text/javascript">
      $(function() {
        $('.third-item').siblings().css('background-color', 'red');
      })
    </script>
  </head>
  <body>
    <ul>
      <li>list item 1</li>
      <li>list item 2</li>
      <li class="third-item">list item 3</li>
      <li>list item 4</li>
      <li>list item 5</li>
    </ul>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_14.png)

实例：

* 5.1.查找3个列表中，所有黄色li元素的独一无二的的兄弟元素 (如果条件适当的话，还包括其它黄色 li 元素)。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      ul {
        float: left;
        margin: 5px;
        font-size: 16px;
        font-weight: bold;
      }
      p {
        color: blue;
        margin: 10px 20px;
        font-size: 16px;
        padding: 5px;
        font-weight: bolder;
      }
      .hilite {
        background: yellow;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <ul>
      <li>One</li>
      <li>Two</li>
      <li class="hilite">Three</li>
      <li>Four</li>
    </ul>
    <ul>
      <li>Five</li>
      <li>Six</li>
      <li>Seven</li>
    </ul>
    <ul>
      <li>Eight</li>
      <li class="hilite">Nine</li>
      <li>Ten</li>
      <li class="hilite">Eleven</li>
    </ul>
    <p>Unique siblings: <b></b></p>
    <script>
      $(function() {
        var len = $(".hilite").siblings()
          .css("color", "red")
          .length;
        $("b").text(len);
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_15.png)

### 6.prev():获取集合中每个匹配元素紧邻的前一个兄弟元素

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../jquery.min.js"></script>
    <script type="text/javascript">
      $(function() {
        $('.third-item').prev().css('background-color', 'red');
      })
    </script>
  </head>
  <body>
    <ul>
      <li>list item 1</li>
      <li>list item 2</li>
      <li class="third-item">list item 3</li>
      <li>list item 4</li>
      <li>list item 5</li>
    </ul>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_16.png)

举例：

* 6.1.找到每个div紧邻的前一个兄弟元素。

```javascript
<!DOCTYPE html>
<html>
<head>
  <style>
  div { width:40px; height:40px; margin:10px;
        float:left; border:2px blue solid; 
        padding:2px; }
  span { font-size:14px; }
  p { clear:left; margin:10px; }
  </style>
  <script src="../jquery.min.js"></script>
</head>
<body>
  <div></div>
  <div></div>
  <div><span>has child</span></div>
  <div></div>
  <div></div>
  <div></div>
  <div id="start"></div>
  <div></div>
  <p><button>Go to Prev</button></p>
<script>
  $(function(){
    var $curr = $("#start");
    $curr.css("background", "#f99");
    $("button").click(function () {
      $curr = $curr.prev();
      $("div").css("background", "");
      $curr.css("background", "#f99");
    });
 })
</script>
</body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_17.png)

### 7.prevall()：获得集合中每个匹配元素的所有前面的兄弟元素

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../jquery.min.js"></script>
    <script type="text/javascript">
      $(function() {
      $('li.third-item').prevAll().css('background-color', 'red');
      })
    </script>
  </head>
  <body>
    <ul>
      <li>list item 1</li>
      <li>list item 2</li>
      <li class="third-item">list item 3</li>
      <li>list item 4</li>
      <li>list item 5</li>
    </ul>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_18.png)

举例：

* 7.1.找到上一个div之前的所有div并给添加一个类。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        width: 70px;
        height: 70px;
        background: #abc;
        border: 2px solid black;
        margin: 10px;
        float: left;
      }
      div.before {
        border-color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <script>
      $(function() {
        $("div:last").prevAll().addClass("before");
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_19.png)

### 8.next():取得匹配的元素集合中每一个元素紧邻的后面同辈元素的元素集合

```javascript
    <script type="text/javascript">
      $(function() {
      $('li.third-item').next().css('background-color', 'red');
      })
    </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_20.png)

### 9.nextall()：取得匹配的元素集合中每一个元素紧邻的后面同辈元素的元素集合

```javascript
    <script type="text/javascript">
      $(function() {
      $('li.third-item').nextAll().css('background-color', 'red');
      })
    </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_21.png)

`next()`、`nextall()`和`prev()`、`prevall()`使用方法都是相同的，只是表现的结果是不同的。

## 筛选->过滤

### 10.eq():在匹配的集合中选择索引值为index的元素。

实例：

* 10.1.将一个类添加到列表2，第2项，目标是第二个到最后一个

```javascript
<!doctype html>
<html lang="en">

  <head>
    <meta charset="utf-8">
    <title>eq demo</title>
    <style>
      .foo {
        color: blue;
        background-color: yellow;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>

  <body>
    <ul class="nav">
      <li>List 1, item 1</li>
      <li>List 1, item 2</li>
      <li>List 1, item 3</li>
    </ul>
    <ul class="nav">
      <li>List 2, item 1</li>
      <li>List 2, item 2</li>
      <li>List 2, item 3</li>
    </ul>
    <script>
      $(function() {
        $("li:eq(-2)").addClass("foo");
      })
    </script>

  </body>

</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_22.png)

* 10.2.

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>eq demo</title>
    <script src="../../js/jquery.min.js"></script>
  </head>
  <body>
    <ul class="nav">
      <li>List 1, item 1</li>
      <li>List 1, item 2</li>
      <li>List 1, item 3</li>
    </ul>
    <ul class="nav">
      <li>List 2, item 1</li>
      <li>List 2, item 2</li>
      <li>List 2, item 3</li>
    </ul>
    <script>
      $(function() {
        $("ul.nav li:eq(1)").css("backgroundColor", "#ff0");
        $("ul.nav").each(function(index) {
          $(this).find("li:eq(1)").css("fontStyle", "italic");
        });
        $("ul.nav li:nth-child(2)").css("color", "red");
      })
    </script>
  </body>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_23.png)

### 11.hasClass():确定指定的元素是否有给定的类

如果匹配元素上有指定的样式，那么`.hasClass() `方法将返回true ，即使元素上可能还有其他样式。

```javascritp

<div id="mydiv" class="class1 class2"></div>

$('#mydiv').hasClass('class1')    //true


$('#mydiv').hasClass('class2')    //true

$('#mydiv').hasClass('class3')    //true

```

举例：在匹配的元素上寻找 `'selected'` 样式

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      p {
        margin: 8px;
        font-size: 16px;
      } 
      .selected {
        color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p>This paragraph is black and is the first paragraph.</p>
    <p class="selected">This paragraph is red and is the second paragraph.</p>
    <div id="result1">First paragraph has selected class: </div>
    <div id="result2">Second paragraph has selected class: </div>
    <div id="result3">At least one paragraph has selected class: </div>
    <script>
      $("div#result1").append($("p:first").hasClass("selected").toString());
      $("div#result2").append($("p:last").hasClass("selected").toString());
      $("div#result3").append($("p").hasClass("selected").toString());
   </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_24.png)

### 12.map()：通过一个函数匹配当前集合中的每个元素,产生一个包含新的jQuery对象。


注意：`map()`方法有返回值，会产生一个新的集合，而`each()`方法没有返回值。

`map()`方法的使用形式：

```javascript
第一种形式的用法：

var array=[];

$.map(array,function(object,index){})==>全局对象的方法

//参数的顺序不同，需要注意下

第二种形式的用法：获取li的内部结点：

var array=$("li").map(function(index,element){})==实例对象的方法
```


举例：

* 12.1.第一种形式，操作数组，存在一个遍历的过程。(显式迭代)

```javascript
        <script>
            $(function(){
                var array=["a","b","c"];
                var arr=$.map(array,function(obj,index){
                    console.log(index);//数组的索引值
                    return obj+"-obj";
                })
                for(var i=0;i<arr.length;i++){
                    console.log(arr[i]);
                }
            })
        </script>
```


* 12.2.举例第二种形式：存在一个遍历的过程，(显示迭代)

```
<body>
  <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
  </ul>
</body>
<script>
     $(function(){
         $("li").map(function(index,element){
            console.log(index);//数组的索引值
             $(element).css("background-color","#878080");
             })
         })
  </script>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_39.png)

注意这两种形式的写法的数的顺序有些不同，但是index都是表示的索引值。

实例：

* 12.3.获取元素集合的值

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../jquery.min.js"></script>
    <script>
      $(function() {
        var string=$(':checkbox').map(function() {
          return this.id;
        }).get().join();
        console.log(string);
      })
    </script>
  </head>
  <body>
    <form method="post" action="">
      <fieldset>
        <div>
          <label for="two">2</label>
          <input type="checkbox" value="2" id="two" name="number[]">
        </div>
        <div>
          <label for="four">4</label>
          <input type="checkbox" value="4" id="four" name="number[]">
        </div>
        <div>
          <label for="six">6</label>
          <input type="checkbox" value="6" id="six" name="number[]">
        </div>
        <div>
          <label for="eight">8</label>
          <input type="checkbox" value="8" id="eight" name="number[]">
        </div>
      </fieldset>
    </form>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_25.png)

* 12.4.获取表单中所有表单元素的值。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <style>
      p {
        color: red;
      }
  </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p><b>Values: </b></p>
    <form>
      <input type="text" name="name" value="张三" />
      <input type="text" name="password" value="123" />
      <input type="text" name="url" value="https://yingyo.github.io" />
    </form>
    <script>
      $("p").append($("input").map(function() {
        return $(this).val();
      }).get().join(", "));
    </script>

  </body>

</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_26.png)

## 筛选->串联

Jquery的又一大特点：链式编程

### 13.end()：返回到作用域链被破坏的前一个状态

终止在当前链的最新过滤操作，并返回匹配的元素的前一个状态

```javascript
<body>
<div>
  <p>我是div内部的p元素
      <span>我是div内部的p元素的内部的span元素</span>
    </p>
</div>
</body>
<script>
    $(document).ready(function(){
        $("div")
                .children("p")
                .children("span")
                .css("background-color","red")
                .end()
                .css("background-color","pink")
        })
        </script>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_37.png)

实例：

* 13.1.选择所有的段落，在其中查找span元素，之后再恢复到选择段落的状态。

```javascript
<!DOCTYPE html>
<html>
  <head>
    <style>
      p,
      div {
        margin: 1px;
        padding: 1px;
        font-weight: bold;
        font-size: 16px;
      }
      div {
        color: blue;
      }
      b {
        color: red;
      }
    </style>
    <script src="http://code.jquery.com/jquery-latest.js"></script>
  </head>
  <body>
    <p>
      Hi there <span>how</span> are you <span>doing</span>?
    </p>
    <p>
      This <span>span</span> is one of several <span>spans</span> in this
      <span>sentence</span>.
    </p>
    <div>
      Tags in jQuery object initially: <b></b>
    </div>
    <div>
      Tags in jQuery object after find: <b></b>
    </div>
    <div>
      Tags in jQuery object after end: <b></b>
    </div>
    <script>
      jQuery.fn.showTags = function(n) {
        var tags = this.map(function() {
            return this.tagName;
          })
          .get().join(", ");
        $("b:eq(" + n + ")").text(tags);
        return this;
      };
      $("p").showTags(0)
        .find("span")
        .showTags(1)
        .css("background", "yellow")
        .end()
        .showTags(2)
        .css("font-style", "italic");
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_27.png)