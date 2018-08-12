---
title: jQuery选择器详解
comments: true
date: 2018-05-02 19:20:42
categories: 博客列表
tags: JavaScript框架
about:

---

> 在学习JQuery时，各类选择器是不是已经让你晕头转向，那你就来对地方了，相信大家看了这篇文章之后，一定会对JQuery的各类选择权有较为清晰的认识。


JQuery的选择器可谓是五花八门，让人们傻傻的分不清，这篇文章就来捋一捋吧。

```JavaScript

<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            div,
            span
            {
                width: 140px;
                height: 140px;
                margin: 5px;
                background: #aaa;
                border: #000 1px solid;
                float: left;
                font-size: 17px;
                font-family: Verdana;
            }
            
            div.mini {
                width: 55px;
                height: 55px;
                background-color: #aaa;
                font-size: 12px;
            }
            
            div.hide {
                display: none;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            
            })
        </script>
    </head>
    <body>
        <div class="one" id="one">
            id为one,class为one的div
            <div class="mini"> class为mini</div>
        </div>
        <div class="one" id="two" title="test">
            id为two,class为one,title为test的div
            <div class="mini" title="other">class为mini,title为other</div>
            <div class="mini" title="test">class为mini,title为test</div>
        </div>
        <div class="one">
            <div class="mini">class为mini</div>
            <div class="mini">class为mini</div>
            <div class="mini">class为mini</div>
            <div class="mini"></div>
        </div>
        <div class="one">
            <div class="mini">class为mini</div>
            <div class="mini">class为mini</div>
            <div class="mini">class为mini</div>
            <div class="mini" title="tesst">class为mini,title为tesst</div>

        </div>
        <div style="display: none;" class="none">
            style的display为"none"的div
        </div>
        <div class="hide">class为hide的div</div>
        <div>
            包含input的type为"hidden"的div
            <input type="hidden" size="8" />
        </div>
        <span>span标签</span>
    </body>
</html>
```

上面的代码运行的结果如下图所示，把该图看做是没有任何选择器的默认状态。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_1.png)


## Basics：基本选择器

* `element`：元素选择器：根据给定的元素名匹配元素

```JavaScript

//选择元素名是div的所有元素
$(document).ready(function(){
$("div").css("background-color","chartreuse");
})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_2.png)

* `#id`：ID选择器：根据给定的ID匹配一个元素

```JavaScript

//选择id为one的元素
$(document).ready(function(){   
      $("#one").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_3.png)

* `.class`：类选择器：根据给定的类名匹配元素

```JavaScript

//选择class为mini的所有元素
$(document).ready(function(){   
        $(".mini").css("background-color","chartreuse");
            })
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_4.png)

* `*`：通配符选择器：根据给定的元素名匹配元素

```JavaScript

//选择所有的元素
$(document).ready(function(){
    $("*").css("background-color","chartreuse");
       })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_5.png)


* `selector1,selector2,selectorN`：并集选择器：将每一个选择器匹配到的元素合并后一起返回

```JavaScript

//选择所有的span元素和id为two的div元素
$(document).ready(function(){       
    $("span,#two").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_6.png)

* `selector1 selector2`：交集选择器：将每一个选择器匹配到的元素相交后一起返回

```JavaScript

//选择父元素的id为one，子元素的class为mini的元素
$(document).ready(function(){   
    $("#one, .mini").css("background-color","chartreuse");
            })
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_7.png)


## Hierarchy：层次选择器

* `ancestor descendant`：后代选择器，会选中标签名是div的下面的元素，不仅可以是一层，也可以是多层。

```JavaScript

//改变body内的所有div的背景色

$(document).ready(function(){

     $("body div").css("background-color","chartreuse")；

})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_8.png)

* `parent > child`：子元素选择器，选中的必须是子元素，中间不能隔多层。

```JavaScript

//改变body内子div元素的背景色
$(document).ready(function(){
    $("body>div").css("background-color","chartreuse");
})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_9.png)

* `prev + next`：选取紧接在prev元素后的next元素

```JavaScript

//改变class为one的下一个div的背景色
$(document).ready(function(){       

    $(".one+div").css("background-color","chartreuse");
    })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_10.png)


* `prev ~ siblings`：选取prev元素之后的索引siblings元素

```JavaScript

<script type="text/javascript">
//改变id为two的元素后面的所有div同辈元素的背景色.
$(document).ready(function(){       
    $("#two~div").css("background-color","chartreuse");
    })
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_11.png)


## Basic Filters：基本过滤筛选器

* `：first`：选择第一个div元素.

```JavaScript

//改变第一个<div>元素的背景色
$(document).ready(function(){       
    $("div:first").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_12.png)

* `：last`：选择最后一个div元素.

```JavaScript

//改变最后一个<div>元素的背景色
$(document).ready(function(){       
    $("div:last").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_13.png)


* `:not(selector)`：去除所有与给定选择器匹配的元素

```JavaScript

//选择class不为one的所有div元素.
$(document).ready(function(){
    $("div:not(.one)").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_14.png)


* `:even`：选取索引值是偶数的所有元素，索引值从0开始

```JavaScript

//偶数（序号，从零开始）
//选择索引值为偶数 的div元素。
$(document).ready(function(){
    $("div:even").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_15.png)


* `:odd`：选取索引值是基数的所有元素，索引值从0开始

```JavaScript

/奇数（序号，从零开始）
//选择 索引值为奇数 的div元素
$(document).ready(function(){   

    $("div:odd").css("background-color","chartreuse");  
})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_16.png)


* `:eq(index)`：选取索引值等于index的元素

```JavaScript

//等于（序号，从零开始）
//选择索引等于 3 的元素
    $(document).ready(function(){
        $("div:eq(3)").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_17.png)

* `：gt(index)`：选取索引值大于index的元素

```JavaScript

//大于（序号，从零开始）
//选择 索引大于 3 的元素
$(document).ready(function(){
    $("div:gt(3)").css("background-color","chartreuse");
       })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_18.png)


* `:lt(index)`：选取索引值小于index的元素

```JavaScript

//小于（序号，从零开始）

//选择索引小于 3 的元素
$(document).ready(function(){       
    $("div:lt(3)").css("background-color","chartreuse");
})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_19.png)

* `:header`：选取所有的标题元素

```JavaScript

//选择所有的标题元素.比如h1, h2, h3等等...

$(document).ready(function(){   
    $(":header").css("background-color","chartreuse");
})
```

* `：animated`：选取当前正在执行动画的所有元素

```JavaScript

//改变当前正在执行动画的元素的背景色. 
$(document).ready(function(){       
    $(":animated").css("background-color","chartreuse");
            })
```

* `：focus`：选取当前获取焦点的元素

```JavaScript

//改变当前获取焦点的元素的背景色
$(document).ready(function(){   
        $(":focus").css("background-color","chartreuse");
            })
```

## Content Filters ：内容过滤选择器


* `：contains(text)`：选择含有文本内容为"text"的元素。

```JavaScript

//选取含有文本"di"的div元素.
    $(document).ready(function(){
        $("div:contains(di)").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_20.png)

* `：empty`：选择不包含子元素或者文本的空元素

```JavaScript

   
//改变不包含子元素(或者文本元素)的div空元素的背景色.
    $(document).ready(function(){
        $(":empty").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_21.png)


* `：has(selector)`：选取含有选择器所匹配的元素的元素

```JavaScript
 
//改变含有class为mini元素 的div元素的背景色.
$(document).ready(function(){   
    $("div:has(.mini)").css("background-color","chartreuse");
            })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_22.png)


* `：parent`：选取含有子元素或者文本的元素


```JavaScript
   
//改变含有子元素(或者文本元素)的div元素的背景色.
$(document).ready(function(){       
    $("div:parent").css("background-color","chartreuse");
})
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_23.png)


## Visibility Filters：可见性过滤选择器


* `：hidden`：选取所有不可见的元素包括`<input type="hidden"/>.`

```JavaScript
 
//显示隐藏的元素并且改变背景色
$(document).ready(function(){     
        $("div:hidden").show(3000).css("background-color","chartreuse");
        })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_24.png)

* `：visible`：选取所有可见的元素.

```JavaScript

//改变所有可见的<div>元素的背景色

$(document).ready(function(){
        $("div:visible").css("background-color","chartreuse");
})
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_25.png)

## Attribute Filters ：属性过滤选择器

* `[attribute]`：选取拥有此属性的元素

```JavaScript

//选取拥有属性id的div元素
$(document).ready(function(){             
$("div[id]").css("background-color","chartreuse");
        })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_26.png)


* `[attribute=value]`：选取属性值为value的元素

```JavaScript
//选取属性title为"test"的div元素
$(document).ready(function(){             
       $("div[title=test]").css("background-color","chartreuse");
        })
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_27.png)


* `[attribute!=value]`：选取属性值不等于value的元素

```JavaScript
//选取属性title不等于"test"的div元素

$(document).ready(function(){            
$("div[title!=test]").css("background-color","chartreuse");
        })
//注意：没有div的元素也会被选取
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_28.png)

下面用一个更详细的例子来说下：

```JavaScript
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <title></title>
        <style>
            div,
            span,
            p {
                width: 140px;
                height: 140px;
                margin: 5px;
                background: #aaa;
                border: #000 1px solid;
                float: left;
                font-size: 17px;
                font-family: Verdana;
            }
            
            div.mini {
                width: 55px;
                height: 55px;
                background-color: #aaa;
                font-size: 12px;
            }
            
            div.hide {
                display: none;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            
        </script>

    </head>

    <body>

        <div title="en">title为en的div元素</div>
        <div title="en-UK">title为en-UK的div元素</div>
        <div title="english">title为english的div元素</div>
        <div title="en uk">title为en uk的div元素</div>
        <div title="uken">title为uken的div元素</div>

    </body>
</html>
```

* `[attribute^=value]`：选取属性值以value开头的元素

```JavaScript

//选取属性title值以en开始div元素.
 $(document).ready(function(){     
        $("div[title^=en]").css("background-color","chartreuse");
                
      });
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_29.png)


* `[attribute$=value]`：选取属性值以value结束的元素

```JavaScript

//选取 属性title值 以en为结尾的div元素.
  $(document).ready(function(){    
        $("div[title$=en]").css("background-color","chartreuse")
  });
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_30.png)

* `[attribute*=value]`：选取属性值含有value的元素

```JavaScript

//任意对个，包含的关系
//选取 属性title值 含有 en  的div元素.
    $(document).ready(function(){
            $("div[title*=en]").css("background-color","chartreuse")
               });
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_31.png)

* `[attribute|=value]`：选取属性值等于给定字符串或以该字符串为前缀的元素

```JavaScript

//选取属性title等于en或以en为前缀（该字符串后跟一个连字符'-'）的元素
  $(document).ready(function(){    
        $("div[title|=en]").css("background-color","chartreuse")
   });
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_32.png)

* `[attribute~=value]`：选取属性用空格分隔的值中包含一个给定的元素

```JavaScript

//选取属性title用空格分隔的值中包含字符uk的元素.
 $(document).ready(function(){    
        $('div[title~=uk]').css("background-color","chartreuse") 
});
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_33.png)

* `[attrSel1][attrSel2][attrSelN]`：用属性选择器合并成一个复合属性选择器，满足多个条件，每次选择一次，缩小一次范围，既满足第一个，又满足第二个，又满足第三个....

```JavaScript

 //选取拥有属性id,并且属性title以"test"结束的<div>元素
  $(document).ready(function(){
        $('div[id][title=test]').css("background-color","chartreuse")
});
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_34.png)

## Child Filter ：子元素过滤选择器（索引从1开始）

* `:first-child`：选取每个父元素下的第一个子元素

```JavaScript

 //改变class为one的<div>父元素下的第一个子元素的背景色
   $(document).ready(function(){
    $('.one :first-child').css("background-color","chartreuse");
        });
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_35.png)


* `:last-child`：选取每个父元素下的最后一个子元素

```JavaScript

   //改变class为one的<div>父元素下的最后一个子元素的背景色
  $(document).ready(function(){
        $('.one :last-child').css("background-color","chartreuse");
   });
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_36.png)


* `：nth-child`：表示的是第几。子元素中的第几个元素

* `：nth-child(even)`：偶数

* `：nth-child(odd)`：奇数

* `：nth-child(2)`：第二个

```JavaScript

//改变每个class为one的<div>父元素下的第二个子元素的背景色
 $(document).ready(function(){          
        $('.one :nth-child(2)').css("background-color","chartreuse")
});
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_37.png)

* `：nth-child(2n)`：偶数倍

* `：nth-child(2n+1)`：基数倍

* `：only-child`：只选中是单独子元素的元素，如果父元素下的仅仅只有一个子元素，那么选中这个子元素

```JavaScript

 //如果class为one的<div>父元素下只有一个子元素，那么则改变这个子元素的背景色
  $(document).ready(function(){    
        $('.one :only-child').css("background-color","chartreuse")
});
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232445/t_38.png)


## Forms：表单选择器

$(':input')：选取所有的`<input>`元素、`<textarea>`元素、`<select>`元素、和`<button>`元素

$(':text')：选取所有的单行文本框

$(':password')：选取所有的密码框

$(':radio')：选取所有的单选框

$(':checkbox')：选取所有的多选框

$(':submit')：选取所有的提交按钮

$(':image')：选取所有的图像按钮

$(':reset')：选取所有的重置按钮

$(':button')：选取所有的按钮

$(':file')：选取所有的上传域

$(':hidden)：选取所有不可见元素

## Form Filters：表单对象属性过滤选择器

$(':enabled')：选取所有可用元素

$(':disabled')：选取所有不可用元素

$(':checked')：选取所有被选中的元素（单选框、复选框）

$(':selected')：选取所有被选中的选项元素（下拉列表）



