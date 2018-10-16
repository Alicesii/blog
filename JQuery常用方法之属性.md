---
title: jQuery常用方法之属性
comments: true
date: 2018-06-19 15:09:31
categories: 前端
tags: JavaScript框架
about:

---

## 属性->属性

### 1.attr()：获取指定的元素集合中的第一个元素的属性的值`(attribute)`。

描述: 和属性相关的操作(在DOM对象中是：setattribute()方法、getAttribute() 方法)

如果传参数就是设置属性值，如果不传参数就是获取属性值。

```javascript

//设置属性值
$('a').attr('title', '百度一下');
$('a').attr('href', 'http://www,baidu.com');

//获取属性值
$('a').attr('title');
$('a').attr('href');
```


举例：

当它发生变化时，显示复选框的属性和属性。

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>attr demo</title>
    <style>
      p {
        margin: 20px 0 0;
      }
      b {
        color: blue;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <input id="check1" type="checkbox" checked="checked">
    <label for="check1">Check me</label>
    <p></p>
    <script>
      $(function() {
        $("input")
          .change(function() {
            var $input = $(this);
            $("p").html(".attr( 'checked' ): <b>" + $input.attr("checked") + "</b><br>" +
              ".prop( 'checked' ): <b>" + $input.prop("checked") + "</b><br>" +
              ".is( ':checked' ): <b>" + $input.is(":checked") + "</b>");
          })
          .change();
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_47.png)

在页面的第一个`<em>`中找到title属性。

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>attr demo</title>
    <style>
      em {
        color: blue;
        font-weight: bold;
      }
      div {
        color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p>Once there was a <em title="huge, gigantic">large</em> dinosaur...</p>
    The title of the emphasis is:
    <div></div>
    <script>
      $(function() {
        var title = $("em").attr("title");
        $("div").text(title);
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_48.png)
为页面中全部的`<img>`
设置一些属性。

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>attr demo</title>
    <style>
      img {
        padding: 10px;
      }
      div {
        color: red;
        font-size: 24px;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <img />
    <img />
    <img />
    <div><b>显示图片的属性</b></div>
    <script>
      $(function(){
      $("img").attr({
        src: "/resources/hat.gif",
        title: "jQuery",
        alt: "jQuery Logo"
      });
      $("div").text($("img").attr("alt"));
    })
    </script>
  </body>
</html>
```

使第二个后面的按钮disabled

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>attr demo</title>
  <style>
  div {
    color: blue;
  }
  span {
    color: red;
  }
  b {
    font-weight: bolder;
  }
  </style>
  <script src="../jquery.min.js"></script>
</head>
<body>

<div>Zero-th <span></span></div>
<div>First <span></span></div>
<div>Second <span></span></div>

<script>
$(function(){
$( "div" )
  .attr( "id", function( arr ) {
    return "div-id" + arr;
  })
  .each(function() {
    $( "span", this ).html( "(id = '<b>" + this.id + "</b>')" );
 });
})
</script>

</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_49.png)

通过图片的title属性设置src属性

```
<!doctype html>
<html lang="en">

  <head>
    <meta charset="utf-8">
    <title>attr demo</title>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <img title="hat.gif">
    <script>
      $(function() {
        $("img").attr("src", function() {
          return "/resources/" + this.title;
        });
      })
    </script>
  </body>
</html>
```


### 2.removeattr()：为指定的元素集合中的每个元素中移除一个属性

举例：点击按钮，添加或删除按钮后面input元素的title属性。

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>removeAttr demo</title>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <button>Change title</button>
    <input type="text" title="hello there">
    <div id="log"></div>
    <script>
      (function() {
        var inputTitle = $("input").attr("title");
        $("button").click(function() {
          var input = $(this).next();

          if(input.attr("title") === inputTitle) {
            input.removeAttr("title")
          } else {
            input.attr("title", inputTitle);
          }
          $("#log").html("input title is now " + input.attr("title"));
        });
      })();
    </script>
  </body>
</html>
```

### 3.prop():获取匹配的元素集中第一个元素的属性(property)值

举例:

Checked属性显示一个复选框，因为它的变化和属性。

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>prop demo</title>
    <style>
      p {
        margin: 20px 0 0;
      }
      b {
        color: blue;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <input id="check1" type="checkbox" checked="checked">
    <label for="check1">Check me</label>
    <p></p>
    <script>
      $(function() {
        $("input").change(function() {
          var $input = $(this);
          $("p").html(
            ".attr( \"checked\" ): <b>" + $input.attr("checked") + "</b><br>" +
            ".prop( \"checked\" ): <b>" + $input.prop("checked") + "</b><br>" +
            ".is( \":checked\" ): <b>" + $input.is(":checked") + "</b>");
        }).change();
      })
    </script>
  </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_47.png)

禁用页面上的所有复选框。

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>prop demo</title>
    <style>
      img {
        padding: 10px;
      }
      div {
        color: red;
        font-size: 24px;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <input type="checkbox" checked="checked">
    <input type="checkbox">
    <input type="checkbox">
    <input type="checkbox" checked="checked">
    <script>
      $(function() {
        $("input[type='checkbox']").prop({
          disabled: true
        });
      })
    </script>
  </body>
</html>
```

注意：attr()函数和prop()函数的区别:

属性：attribute

特性：property

关于 checked attribute要记住的最重要的概念是，它不同于checked property attribute,事实上等同于defaultCheckproperty,并且应该只能用来设置checkbox的初始值。

ckecked attribute值不用用来改变checkbox的状态，但是checked property可以。因此，跨浏览器的途径来确定一个chexkbox是否被选中要使用property.

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_9.png)

下面的三种方式都可以判断复选框是不被选中了，第一种方法是js方式，第二种方式是JQuery方式利用prpo()方法，第三种方式利用JQuery方式的is()
方法来进行判断
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_10.png)

## 属性->CSS类

addClass()：为每个匹配的元素添加指定的样式类名

举例：将一个函数传递给.addClass()，将`"绿色"`类添加到已有“红色”类的div中。

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>addClass demo</title>
    <style>
      div {
        background: white;
      }
      .red {
        background: red;
      }
      .red.green {
        background: green;
      }
    </style>
    <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
  </head>
  <body>
    <div>This div should be white</div>
    <div class="red">This div will be green because it now has the "green" and "red" classes. It would be red if the addClass function failed.</div>
    <div>This div should be white</div>
    <p>There are zero green divs</p>
    <script>
    $(function(){
      $("div").addClass(function(index, currentClass) {
        var addedClass;
        if(currentClass === "red") {
          addedClass = "green";
          $("p").text("There is one green div");
        }
        return addedClass;
      });
    })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_50.png)

removeClass()移除集合中每个匹配元素上一个，多个或全部样式。

举例:添加类样式和删除类样式：

```javascript
 <style type="text/css">
        .div1{
            background-color: darksalmon;
            width: 100px;
            height: 100px;
            margin: 100px auto;
        }
        .div2{
            background-color: #008000;
            width: 500px;
            height: 500px;
            margin: 100px auto;
        }
    </style>
   <div>Hello World</div>
   <script type="text/javascript">
       $(document).ready(function() {
            $('div').addClass('div1');
            $('div').addClass('div1').addClass("div2");
            //可以添加一个或者多个类，但是相同的属性会覆盖
            $("div").removeClass("div2");
    });

```

toggleClass()：为匹配的元素集合中的每个元素上添加或删除一个或多个样式类（class）,取决于这个样式类（class）是否存在或state参数的值。

切换类的效果：

第一种方法：使用toggleClass()方法：

第二种方法：首先判断有没有该类，如果有的话就删除，如果没有的话就添加该类，其实就是一个删除与添加的过程

```javascript
//第二种方法
<style type="text/css">
        .div1{
            background-color: darksalmon;
            width: 100px;
            height: 100px;
            margin: 100px auto;
        }
    </style>
   <div>Hello World</div>
   <input type="button" value="切换" id="btn">
   <script type="text/javascript">
     $(document).ready(function(){
        $("#btn").click(function(){
             if($("div").hasClass("div1")){
                $("div").removeClass("div1");
            }else{
                $("div").addClass("div1");
            }
            })

        })
   </script>

//第一种方法：
<style type="text/css">
        .div1{
            background-color: darksalmon;
            width: 100px;
            height: 100px;
            margin: 100px auto;
        }
    </style>
   <div>Hello World</div>
   <input type="button" value="切换" id="btn">
   <script type="text/javascript">
     $(document).ready(function(){
        $("#btn").click(function(){
            $("div").toggleClass("div1");
            })

        })
   </script>
```

举例：每当第三次点击段落的时候添加 "highlight" 样式类, 第一次和第二次点击的时候移除 "highlight" 样式类

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>toggleClass demo</title>
    <style>
      p {
        margin: 4px;
        font-size: 16px;
        font-weight: bolder;
        cursor: pointer;
      }
      .blue {
        color: blue;
      }
      .highlight {
        background: red;
      }
    </style>
    <script type="text/javascript" src="../../js/jquery-1.11.1.min.js" ></script>
  </head>
  <body>
    <p class="blue">Click to toggle (<span>clicks: 0</span>)</p>
    <p class="blue highlight">highlight (<span>clicks: 0</span>)</p>
    <p class="blue">on these (<span>clicks: 0</span>)</p>
    <p class="blue">paragraphs (<span>clicks: 0</span>)</p>
    <script>
      $(function() {
        var count = 0;
        $("p").each(function() {
          var $thisParagraph = $(this);
          var count = 0;
          $thisParagraph.click(function() {
            count++;
            $thisParagraph.find("span").text("clicks: " + count);
            $thisParagraph.toggleClass("highlight", count % 3 === 0);
          });
        });
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_51.png)

注意：

* 添加的样式一定必须是类样式".div{} "，而不能标签样式"div{}"，或者ID样式"#div{}".

* 操作类样式的时候，所有的类名，都不带点（"."）

> Javascript中有很多方法可以操作DOM对象，Jquery也有很多方法可以操作DOM对象，可是它们之间实现的过程可是差距很大的，通过下面的种种案例我们不难看出使用jQuery的方式来操作DOM更加的简介。

 对比JS操作DOM和jQuery操作DOM

javascript只能用className()这样一个方法来判断存不存在样式、添加样式、删除样式、切换样式。

```javascript

window.onload=function(){
//判断有没有样式
function hasClass(obj,cls){
    return obj.className.match(new RegExp('(\\s|^)'+cls+'(\\s|$)'));
}

//添加样式
function addClass(obj,cls){
    if(!hasClass(obj,cls)){
        obj.className+=" "+cls
    }
}

//删除样式
function removeClass(obj,cls){
    if(hasClass(obj,cls)){
        var reg=new RegExp('(\\s|^)'+cls+'(\\s|$)');
        obj.className=obj.className.replace(reg,' ');
    }
}

//切换样式
function toggleClass(obj,cls){
    if(hasClass(obj,cls)){
        removeClass(obj,cls);
    }else{
        addClass(obj,cls);
    }
  }
}
```

jQuery方式操作对象就简单太多了:


```javascript

$(document).ready(function(){
//判断有没有指定样式
$("#demo").hasClass("newClass");

//添加样式

$("#demo").addClass("newClass");
//删除样式

$("#demo").removeClass("newClass");

//切换样式
$("#demo").toggleClass("newClass");

})
```

## 动态创建元素对比

javascript方式：


```javascript

window.onload=function(){
    //获取div节点
    var demo=document.getElementById("demo");
    //创建a元素
    var aLink=document.createElement("a");
    //创建文本节点
    var aTxt=document.createTextNode("Web前端");

    //把文本节点添加给创建的a元素
    aLink.appendChild(aTxt);
    //给a元素设置属性
    aLink.setAttribute("href","http://www.baidu.com");
    //把动态创建的a元素，添加到获取的div元素中
    demo.appendChild(aLink);
}

```

jQuery方式：创建文本


```javascript
$(document).ready(function(){
     //创建a元素，同时创建属性和文本节点
     var aLink=$("<a href="http://www.baidu.com">Web前端</a>");
    //把动态创建的a元素，添加到获取的div元素中
     $("#demo").append(aLink);
});

```
[有关DOM对象和Jquery对象的区别于联系请看我另一篇博客]()

## 属性：HTML代码/文本/值

val()：获取匹配的元素集合中第一个元素的当前值


获取值的时候，只返回选择到的所有元素的第一个


val()函数中如果传参数就是设置文本框中的值，如果不传参数就是获取文本框中的值。

```javascript
<body>
<input type="button" value="设置val值"/>
<input type="button" value="获取val值"/>
<input type="text"/>
</body>
<script>
$(document).ready(function(){
      //设置文本框的值
   $("input:eq(0)").click(function(){
        $("input:eq(2)").val("我是文本框");
      });
     //获取文本框中的值
     $("input:eq(1)").click(function(){
        var value=$("input:eq(2)").val();
            alert(value);
      });
 })
</script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_5.png)

举例：


从单一列表框和复选列表中取值，并显示选中的值。
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>val demo</title>
    <style>
      p {
        color: red;
        margin: 4px;
      }
      b {
        color: blue;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p></p>
    <select id="single">
      <option>Single</option>
      <option>Single2</option>
    </select>
    <select id="multiple" multiple="multiple">
      <option selected="selected">Multiple</option>
      <option>Multiple2</option>
      <option selected="selected">Multiple3</option>
    </select>
    <script>
      $(function(){
      function displayVals() {
        var singleValues = $("#single").val();
        var multipleValues = $("#multiple").val() || [];
        // When using jQuery 3:
        // var multipleValues = $( "#multiple" ).val();
        $("p").html("<b>Single:</b> " + singleValues +
          " <b>Multiple:</b> " + multipleValues.join(", "));
      }
      $("select").change(displayVals);
      displayVals();
    })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_52.png)
取得文本框的值。
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>val demo</title>
    <style>
      p {
        color: blue;
        margin: 8px;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <input type="text" value="some text">
    <p></p>
    <script>
      $(function() {
        $("input")
          .keyup(function() {
            var value = $(this).val();
            $("p").text(value);
          })
          .keyup();
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_53.png)

设置单一列表框，复选列表，复选框和单选按钮的值。
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>val demo</title>
    <style>
      body {
        color: blue;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <select id="single">
      <option>Single</option>
      <option>Single2</option>
    </select>

    <select id="multiple" multiple="multiple">
      <option selected="selected">Multiple</option>
      <option>Multiple2</option>
      <option selected="selected">Multiple3</option>
    </select>
    <br>
    <input type="checkbox" name="checkboxname" value="check1"> check1
    <input type="checkbox" name="checkboxname" value="check2"> check2
    <input type="radio" name="r" value="radio1"> radio1
    <input type="radio" name="r" value="radio2"> radio2
    <script>
      $(function() {
        $("#single").val("Single2");
        $("#multiple").val(["Multiple2", "Multiple3"]);
        $("input").val(["check1", "check2", "radio1"]);
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_54.png)
将函数作为参数设置文本框的值
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>val demo</title>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p>Type something and then click or tab out of the input.</p>
    <input type="text" value="type something">
    <script>
      $(function() {
        $("input").on("blur", function() {
          $(this).val(function(i, val) {
            return val.toUpperCase();
          });
        });
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_55.png)

html():获取集合中第一个匹配元素的HTML内容

如果传参数就是设置html中的值，如果不传参数就是获取html中的值,如果传了参数，但是参数为空，就相当于删除了这个节点，相当于remove()的效果。

```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
<div>
<p>我是p元素</p>
</div>
<input type="button" value="设置html值"/>
<input type="button" value="获取html值"/>
</body>
<script src="../js/jquery-1.11.1.min.js" type="text/javascript"></script>
<script type="text/javascript">
    $(document).ready(function () {
        $("input:eq(0)").click(function(){
         //设置html内容
        //$("div").html("我是html内容");
       //将原来的div中的标签元素获取到，然后在加上新的标签元素，不会造成覆盖的效果,类似于append的效果，这是最常用的形式。
          var html=$("div").html();
         $("div").html(html+"<h1>我是标题</h1>")
      });
      //获取html内容
      $("input:eq(1)").click(function(){
        var html=$("div").html();
            alert(html);
      });
  });
</script>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_6..png)

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_6.png)

注意：使用html()来创建dom的方式效率比较高，远大于document.createElement();和append();

举例：


点击段落将HTML转化为文本

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>html demo</title>
    <style>
      p {
        margin: 8px;
        font-size: 20px;
        color: blue;
        cursor: pointer;
      }
      b {
        text-decoration: underline;
      }
      button {
        cursor: pointer;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p>
      <b>Click</b> to change the <span id="tag">html</span>
    </p>
    <p>
      to a <span id="text">text</span> node.
    </p>
    <p>
      This <button name="nada">button</button> does nothing.
    </p>
    <script>
      $(function() {
        $("p").click(function() {
          var htmlString = $(this).html();
          $(this).text(htmlString);
        });
      })
    </script>
  </body>
</html>
```


为每个div设置一些内容

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>html demo</title>
    <style>
      .red {
        color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <span>Hello</span>
    <div></div>
    <div></div>
    <div></div>
    <script>
      $(function() {
        $("div").html("<span class='red'>Hello <b>Again</b></span>");
      })
    </script>
  </body>
</html>
```
添加了一些html到每个div，然后立刻做进一步的操作来插入的HTML。
```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>html demo</title>
    <style>
      div {
        color: blue;
        font-size: 18px;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <div></div>
    <div></div>
    <div></div>
    <script>
      $(function(){
      $("div").html("<b>Wow!</b> Such excitement...");
      $("div b")
        .append(document.createTextNode("!!!"))
        .css("color", "red");
      })
    </script>
  </body>
</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_56.png)

text():得到匹配元素集合中每个元素的合并文本，包括他们的后代，也就是说，获取文本内容，获取标签元素的内容，但不包含标签。

```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
    </head>
    <body>
        <div>
            <p>我是p元素</p>
        </div>
        <input type="button" value="设置text值" />
        <input type="button" value="获取text值" />
    </body>
    <script src="../js/jquery-1.11.1.min.js" type="text/javascript"></script>
    <script type="text/javascript">
        $(document).ready(function() {
            $("input:eq(0)").click(function() {
                //设置text内容
                //$("div").text("我是标题");
                $("div").text("<h1>我是标题</h1>");
            });
            //获取text内容
            $("input:eq(1)").click(function() {
                var text = $("div").text();
                alert(text);
            });
        });
    </script>
</html>
```


![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_7.png)

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_8.png)

举例：

在第一段中找到文本（去掉html），然后设置最后一段的html以显示它只是文本（`<b>`标签地作用已经消失）

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>text demo</title>
    <style>
      p {
        color: blue;
        margin: 8px;
      }
      b {
        color: red;
      }
    </style>
    <script src="../jquery.min.js"></script>
  </head>
  <body>
    <p><b>Test</b> Paragraph.</p>
    <p></p>
    <script>
      $(function() {
        var str = $("p:first").text();
        $("p:last").html(str);
      })
  </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_58.png)

在段落中添加文本。这个`<b>`标签将从HTML中脱离出来，也就是说现在的`<b>`标签被当做字符串原样输出了。

```javascript
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>text demo</title>
    <style>
      p {
        color: blue;
        margin: 8px;
      }
    </style>
    <script src=" ../jquery.min.js"></script>
  </head>
  <body>
      <p>Test Paragraph.</p>
    <script>
      $(function() {
        $("p").text("<b>Some</b> new text.");
      })
    </script>
  </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_59.png)