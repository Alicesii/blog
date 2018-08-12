---
title: jQuery常用方法之效果
comments: true
date: 2018-05-16 14:00:31
categories: 博客列表
tags: JavaScript框架
about:

---

## 效果->基本

### 1.show()：显示隐藏的指定的元素

注意：关于动画的API都有一个共同点，就是如果提供回调函数参数，回调函数会在动画完成的时候调用。也就是说，如果该方法的参数中有函数,那么这个函数会等到动画执行完之后才执行。

举例：

* 1.1.缓慢地显示所有隐藏的段落，600毫秒内完成的动画。

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
        <button>Show it</button>
        <p style="display: none">Hello 2</p>
        <script>
            $(function() {
                $("button").click(function() {
                    $("p").show("slow");
                });
            })
        </script>
    </body>
</html>
```
* 1.2.依次显示 div，每个用时200毫秒。一个动画完成后立即开始下一个。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background: #def3ca;
                margin: 3px;
                width: 80px;
                display: none;
                float: left;
                text-align: center;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button id="showr">Show</button>
        <button id="hidr">Hide</button>
        <div>Hello 3,</div>
        <div>how</div>
        <div>are</div>
        <div>you?</div>
        <script>
            $(function(){
            $("#showr").click(function() {
                $("div").first().show("fast", function showNext() {
                    $(this).next("div").show("fast", showNext);
                });
            });
            $("#hidr").click(function() {
                $("div").hide(1000);
            });
        })
        </script>
    </body>
</html>

```

* 1.3.动画显示所有span和input元素。在文本框中输入yes并按回车,会显示一些内容。

```javascript

<!DOCTYPE html>
<html>
    <head>
        <style>
            span {
                display: none;
            }
            div {
                display: none;
            }
            p {
                font-weight: bold;
                background-color: #fcd;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button>Do it!</button>
        <span>Are you sure? (type 'yes' if you are) </span>
        <div>
            <form>
                <input type="text" value="" />
            </form>
        </div>
        <p style="display:none;">I'm hidden...</p>
        <script>
            $(function() {
                function doIt() {
                    $("span,div").show("slow");
                }
                $("button").click(doIt);
                $("form").submit(function() {
                    if($("input").val() == "yes") {
                        $("p").show(4000, function() {
                            $(this).text("Ok, DONE! (now showing)");
                        });
                    }
                    $("span,div").hide("fast");
                    return false;
                });
            })
        </script>
    </body>
</html>
```

### 2.hide()：隐藏指定的元素

举例：

* 2.1.点击链接隐藏所有段落。

```javscript
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>hide demo</title>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>Hello</p>
        <a href="#">Click to hide me too</a>
        <p>Here is another paragraph</p>
        <script>
            $(function() {
                $("p").hide();
                $("a").click(function(event) {
                    event.preventDefault();
                    $(this).hide();
                });
            })
        </script>
    </body>
</html>
```

* 2.2.缓慢的隐藏所有显示的段落，600毫秒内完成的动画。

```javscript
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>hide demo</title>
        <style>
            p {
                background: #dad;
                font-weight: bold;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button>隐藏</button>
        <p>我是P元素</p>
        <p>我也是p元素</p>
        <script>
            $(function() {
                $("button").click(function() {
                    $("p").hide("slow");
                });
            })
        </script>
    </body>
</html>
```

* 2.3.快速隐藏所有的span元素，用时200毫秒。一个结束后立即开始下一个。

```javascript
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>hide demo</title>
        <style>
            span {
                background: #def3ca;
                padding: 3px;
                float: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button id="hidr">Hide</button>
        <button id="showr">Show</button>
        <div>
            <span>Once</span> <span>upon</span> <span>a</span>
            <span>time</span> <span>there</span> <span>were</span>
            <span>three</span> <span>programmers...</span>
        </div>
        <script>
            $(function() {
                $("#hidr").click(function() {
                    $("span:last-child").hide("fast", function() {
                        $(this).prev().hide("fast", arguments.callee);
                    });
                });
                $("#showr").click(function() {
                    $("span").show(2000);
                });
            })
        </script>
    </body>
</html>
```
* 2.4.当点击div之后用2秒时间隐藏这个div。当它完全隐藏后将其移除。

```javascript
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>hide demo</title>
        <style>
            div {
                background: #ece023;
                width: 30px;
                height: 40px;
                margin: 2px;
                float: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <div></div>
        <script>
        $(function(){
            for(var i = 0; i < 5; i++) {
                $("<div>").appendTo(document.body);
            }
            $("div").click(function() {
                $(this).hide(2000, function() {
                    $(this).remove();
                });
            });
        })
        </script>
    </body>
</html>
```

## 效果->淡入淡出

### 3.fadeIn()：通过淡入的方式显示指定元素

举例：

* 3.1.淡出所有段落，在600毫秒内完成这些动画。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            span {
                color: red;
                cursor: pointer;
            }
            div {
                margin: 3px;
                width: 80px;
                display: none;
                height: 80px;
                float: left;
            }
            
            div#one {
                background: #f00;
            }
            
            div#two {
                background: #0f0;
            }
            
            div#three {
                background: #00f;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <span>Click here...</span>
        <div id="one"></div>
        <div id="two"></div>
        <div id="three"></div>
        <script>
            $(function() {
                $(document.body).click(function() {
                    $("div:hidden:first").fadeIn("slow");
                });
            })
        </script>
    </body>
</html>
```

* 3.2.在文本上淡入一个红色的div。一旦动画完成，被div遮盖的文本内容消失。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                position: relative;
                width: 400px;
                height: 90px;
            }
            div {
                position: absolute;
                width: 400px;
                height: 65px;
                font-size: 36px;
                text-align: center;
                color: yellow;
                background: red;
                padding-top: 25px;
                top: 0;
                left: 0;
                display: none;
            }
            span {
                display: none;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>
            Let it be known that the party of the first part and the party of the second part are henceforth and hereto directed to assess the allegations for factual correctness... (
            <a href="#">click!</a>)
            <div><span>CENSORED!</span></div>
        </p>
        <script>
            $(function() {
                $("a").click(function() {
                    $("div").fadeIn(3000, function() {
                        $("span").fadeIn(100);
                    });
                    return false;
                });
            })
        </script>
    </body>
</html>
```

### 4.fadeOut()：通过淡出的方式显示指定元素

举例：

* 4.1.让所有段落渐渐消失，用时 600 毫秒。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                font-size: 150%;
                cursor: pointer;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>
            If you click on this paragraph you'll see it just fade away.
        </p>
        <script>
            $("p").click(function() {
                $("p").fadeOut("slow");
            });
        </script>
    </body>
</html>
```
* 4.2.将你点击的 span 淡出隐藏。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta  charset="utf-8"/>
        <style>
            span {
                cursor: pointer;
            }
            span.hilite {
                background: yellow;
            }
            div {
                display: inline;
                color: red;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <h3>寻找被隐藏的span元素<div></div></h3>
        <p>
            If you <span>really</span> want to go outside
            <span>in the cold</span> then make sure to wear your <span>warm</span> jacket given to you by your <span>favorite</span> teacher.
        </p>
        <script>
            $(function() {
                $("span").click(function() {
                    $(this).fadeOut(1000, function() {
                        $("div").text("'" + $(this).text() + "' has faded!");
                        $(this).remove();
                    });
                });
                $("span").hover(function() {
                    $(this).addClass("hilite");
                }, function() {
                    $(this).removeClass("hilite");
                });
            })
        </script>
    </body>
</html>
```
### 5.fadeTo()：淡入到某个指定的不透明度值

举例：

* 5.1.把第一个段落的透明度渐变成 0.33 (`33%`，大约三分之一透明度), 用时 600 毫秒。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>
            Click this paragraph to see it fade.
        </p>
        <p>
            Compare to this one that won't fade.
        </p>
        <script>
            $(function() {
                $("p:first").click(function() {
                    $(this).fadeTo("slow", 0.33);
                });
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_42.png)

*  5.2.每次点击后把 div 渐变成随机透明度，用时200毫秒。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                width: 80px;
                margin: 0;
                padding: 5px;
            }
            
            div {
                width: 40px;
                height: 40px;
                position: absolute;
            }
            
            div#one {
                top: 0;
                left: 0;
                background: #f00;
            }
            
            div#two {
                top: 20px;
                left: 20px;
                background: #0f0;
            }
            
            div#three {
                top: 40px;
                left: 40px;
                background: #00f;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p></p>

        <div id="one"></div>
        <div id="two"></div>
        <div id="three"></div>
        <script>
            $("div").click(function() {
                $(this).fadeTo("fast", Math.random());
            });
        </script>
    </body>
</html>
```
* 5.3.找到正确答案！渐变耗时250毫秒，并且在完成后改变字体样式。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div,
            p {
                width: 80px;
                height: 40px;
                top: 0;
                margin: 100px;
                position: absolute;
                padding-top: 20px;
            }
            p {
                background: #fcc;
                text-align: center;
            }
            div {
                background: blue;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p>Wrong</p>
        <div></div>
        <p>Wrong</p>
        <div></div>
        <p>Right!</p>
        <div></div>
        <script>
            $(function() {
                var getPos = function(n) {
                    return(Math.floor(n) * 90) + "px";
                };
                $("p").each(function(n) {
                    var r = Math.floor(Math.random() * 3);
                    var tmp = $(this).text();
                    $(this).text($("p:eq(" + r + ")").text());
                    $("p:eq(" + r + ")").text(tmp);
                    $(this).css("left", getPos(n));
                });
                $("div").each(function(n) {
                        $(this).css("left", getPos(n));
                    })
                    .css("cursor", "pointer")
                    .click(function() {
                        $(this).fadeTo(250, 0.25, function() {
                            $(this).css("cursor", "")
                                .prev().css({
                                    "font-weight": "bolder",
                                    "font-style": "italic"
                                });
                        });
                    });
            })
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_43.png)

### 6.fadetoggle()：显示或隐藏指定元素的动画

举例：

* 6.1.第一段落渐隐或渐显，用时600毫秒，并且是线性缓冲效果。而最后一个段落渐隐渐显用时200毫秒, 并且在每次动画完成后插入一个"finished"。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button>fadeToggle p1</button>
        <button>fadeToggle p2</button>
        <p>This paragraph has a slow, linear fade.</p>
        <p>This paragraph has a fast animation.</p>
        <div id="log"></div>
        <script>
            $(function() {
                $("button:first").click(function() {
                    $("p:first").fadeToggle("slow", "linear");
                });
                $("button:last").click(function() {
                    $("p:last").fadeToggle("fast", function() {
                        $("#log").append("<div>finished</div>");
                    });
                });
            })
        </script>
    </body>
</html>
```

小剧场问题：为什么`toggleClass()`和`fadetoggle()`中toggle的位置一个在前一个在后？

因为动词总是跟在名词后面，Class是名词，fade是动词。toggle既可以当做名词又可以当做动词，这个问题其实一点也不重要。


## 效果->滑动

### 7.slidedown()：卷帘门的效果显示指定的元素

注意：`slidedown()`方法一定是展示出来的意思，如果"图片"的位置不同，那么展示的效果也会不同，比如说如果图片的位置在右下角，那就是从下往上面展示，如果图片在左上角，那么就是从上往下展示。

举例：

* 7.1.用600毫秒让所有的div下滑显示出来。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background: #de9a44;
                margin: 3px;
                width: 80px;
                height: 40px;
                display: none;
                float: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        Click me!
        <div></div>
        <div></div>
        <div></div>
        <script>
            $(function() {
                $(document.body).click(function() {
                    if($("div:first").is(":hidden")) {
                        $("div").slideDown("slow");
                    } else {
                        $("div").hide();
                    }
                });
            })
        </script>
    </body>
</html>
```

* 7.2.用1000毫秒让所有文本框组件下滑显示出来。动画完成后改变这些文本框的外观，并让中间的文本框获得输入焦点。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background: #cfd;
                margin: 3px;
                width: 50px;
                text-align: center;
                float: left;
                cursor: pointer;
                border: 2px outset black;
                font-weight: bolder;
            }
            
            input {
                display: none;
                width: 120px;
                float: left;
                margin: 10px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <div>Push!</div>
        <input type="text" />
        <input type="text" class="middle" />
        <input type="text" />
        <script>
            $(function() {
                $("div").click(function() {
                    $(this).css({
                        borderStyle: "inset",
                        cursor: "wait"
                  });
                 $("input").slideDown(1000, function() {
                     $(this).css("border", "2px red inset")
                         .filter(".middle")
                         .css("background", "yellow")
                         .focus();
                      $("div").css("visibility", "hidden");
                    });
                });
            })
        </script>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_44.png)

### 8.slideup()：卷帘门的效果隐藏指定的元素

举例：

* 8.1.让所有的div向上滑，用时400毫秒。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background: #3d9a44;
                margin: 3px;
                width: 80px;
                height: 40px;
                float: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        Click me!
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <script>
            $(function() {
                $(document.body).click(function() {
                    if($("div:first").is(":hidden")) {
                        $("div").show("slow");
                    } else {
                        $("div").slideUp();
                    }
                });
            })
        </script>
    </body>
</html>
```
* 8.2.用200毫秒让父元素滑动收起。动画完成后，显示一个提示信息。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                margin: 2px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <div>
            <button>Hide One</button>
            <input type="text" value="One" />
        </div>
        <div>
            <button>Hide Two</button>
            <input type="text" value="Two" />
        </div>
        <div>
            <button>Hide Three</button>
            <input type="text" value="Three" />
        </div>
        <div id="msg"></div>
        <script>
            $(function() {
                $("button").click(function() {
                    $(this).parent().slideUp("slow", function() {
                        $("#msg").text($("button", this).text() + " has completed.");
                    });
                });
            })
        </script>
    </body>
</html>
```
### 9.slidetoggle()：卷帘门的效果显示或者隐藏指定元素

举例：

* 9.1.让所有的段落滑上或滑下，用时600毫秒。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            p {
                width: 400px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body><button>Toggle</button>
        <p>This is the paragraph to end all paragraphs. You should feel <em>lucky</em>to have seen such a paragraph in your life. Congratulations! </p>
        <script>
            $(function() {
                $("button").click(function() {
                        $("p").slideToggle("slow");
                    }

                );
            })
        </script>
    </body>
</html>
```
* 9.2.让隔开的div交替出现或消失。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background: #b977d1;
                margin: 3px;
                width: 60px;
                height: 60px;
                float: left;
            }
            
            div.still {
                background: #345;
                width: 5px;
            }
            
            div.hider {
                display: none;
            }
            
            span {
                color: red;
            }
            
            p {
                clear: left;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <div></div>
        <div class="still"></div>
        <div style="display:none;">
        </div>
        <div class="still"></div>
        <div></div>
        <div class="still"></div>
        <div class="hider"></div>
        <div class="still"></div>
        <div class="hider"></div>
        <div class="still"></div>
        <div></div>
        <p><button id="aa">Toggle</button> There have been <span>0</span> toggled divs.</p>
        <script>
         $(function(){
            $("#aa").click(function() {
                $("div:not(.still)").slideToggle("slow", function() {
                    var n = parseInt($("span").text(), 10);
                    $("span").text(n + 1);
                });
            });
         })
        </script>
    </body>
</html>
```
## 效果->自定义动画

### 10.animate():自定义动画

描述：

* 在该方法中可以传CSS的属性值，但必须是键值对的形式，

* 所有用于动画的属性必须是数字的，除非另有说明；这些属性如果不是数字的将不能使用基本的jQuery功能。（例如，width, height或者left可以执行动画，但是background-color不能，除非使用jQuery.Color插件。属性值的单位像素（px）,除非另有说明。单位em 和%需要指定使用。

* 动画属性也可以是一个相对值如果提供一个以`+=`或`-=`开始的值，那么目标值就是以这个属性的当前值加上或者减去给定的数字来计算的。

```javascript
<!DOCTYPE html >
<html>

    <head>
        <title></title>
    </head>
    <style type="text/css">
        .block {
            position: relative;
            background-color: Red;
            width: 48px;
            height: 48px;
            float: left;
            margin: 5px;
        }
        .result{
            width: 50px;
            height: 50px;
            font-size: 10px;
            background-color: #E9967A;
            border-width: medium;
        }
    </style>
    <body>
        <div class="result">animate
        </div>
        <button class="btn1">
        Click 1</button>
        <button class="btn2">
        Click 2</button>
        <button class="btn3">
        Click 3</button>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
    </body>
    <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
    <script type="text/javascript">
        $(document).ready(function() {
            $('.btn1').click(function() {
                $('.result').animate({
                    width: "100%",
                    height: "100%",
                    fontSize: "10em",
                    borderWidth: 10
                }, 1000);
            });
            $('.btn2').click(function() {
                $('.result').animate({
                    width: "+=30px",
                    height: "-=30px"
                }, 1000);
            });
            $('.btn3').click(function() {
                $('.block:first').animate({
                    left: 100
                }, {
                    duration: 1000,
                    step: function(now, fx) {
                        $('.block:gt(0)').css('left', now);
                    }
                });
            });
        });
    </script>
</html>
```
注意：如果`animate()`方法后面`有function()`，那么内部就一定要写function的代码，而不能只写表达式的代码。如下所示。

```javascript
<script type="text/javascript">
            $(document).ready(function(){
                $("input").click(function(){
                    $("div").animate(function(){
                    })
                })
            })
        </script>

```

如果`animate()`方法后面没有跟着`function()`，那么内部既可以写function的代码，也可以写表达式的代码。如下所示。

```javascript
<script type="text/javascript">
            $(document).ready(function(){
                $("input").click(function(){
                    $("div").animate({
                    })
                })
            })
        </script>
```
举例：

* 10.1点击按钮，根据指定的一系列属性，在div上应用动画。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                background-color: #bca;
                width: 100px;
                border: 1px solid green;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button id="go">&raquo; Run</button>
        <div id="block">Hello!</div>
        <script>
            $(function() {
                $("#go").click(function() {
                    $("#block").animate({
                        width: "70%",
                        opacity: 0.4,
                        marginLeft: "0.6in",
                        fontSize: "3em",
                        borderWidth: "10px"
                    }, 1500);
                });
            })
        </script>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1238457/o_46.png)

* 10.2.对div应用动画，在left属性上使用相对值。执行动画多次，查看相对值的累加效果。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                position: absolute;
                background-color: #abc;
                left: 50px;
                width: 90px;
                height: 90px;
                margin: 5px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <button id="left">&laquo;</button> <button id="right">&raquo;</button>
        <div class="block"></div>
        <script>
            $(function() {
                $("#right").click(function() {
                    $(".block").animate({
                        "left": "+=50px"
                    }, "slow");
                });
                $("#left").click(function() {
                    $(".block").animate({
                        "left": "-=50px"
                    }, "slow");
                });
            })
        </script>
    </body>
</html>
```

* 10.3.第一个按钮要执行的动画中，使用了`queue: false `选项，该动画使元素的宽度扩大到了总宽 90%,并且文字大小也变大了。一旦字体大小改变完了，边框的动画就会开始。 第二个按钮要执行的动画中，包含了一系列动画，当前一个动画完成时，后一个动画就会开始。

```
<!DOCTYPE html>
<html>

    <head>
        <style>
            div {
                background-color: #bca;
                width: 200px;
                height: 1.1em;
                text-align: center;
                border: 2px solid green;
                margin: 3px;
                font-size: 14px;
            }
            
            button {
                font-size: 14px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>

    <body>
        <button id="go1">&raquo; Animate Block1</button>
        <button id="go2">&raquo; Animate Block2</button>
        <button id="go3">&raquo; Animate Both</button>

        <button id="go4">&raquo; Reset</button>
        <div id="block1">Block1</div>
        <div id="block2">Block2</div>
        <script>
            $(function() {
                $("#go1").click(function() {
                    $("#block1").animate({
                            width: "90%"
                        }, {
                            queue: false,
                            duration: 3000
                        })
                        .animate({
                            fontSize: "24px"
                        }, 1500)
                        .animate({
                            borderRightWidth: "15px"
                        }, 1500);
                });

                $("#go2").click(function() {
                    $("#block2").animate({
                            width: "90%"
                        }, 1000)
                        .animate({
                            fontSize: "24px"
                        }, 1000)
                        .animate({
                            borderLeftWidth: "15px"
                        }, 1000);
                });

                $("#go3").click(function() {
                    $("#go1").add("#go2").click();
                });

                $("#go4").click(function() {
                    $("div").css({
                        width: "",
                        fontSize: "",
                        borderWidth: ""
                    });
                });
            })
        </script>

    </body>

</html>
```
* 10.4.对第一个div的left属性应用动画，在动画执行的过程中，在step函数中改变其余div的left属性。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            div {
                position: relative;
                background-color: #abc;
                width: 40px;
                height: 40px;
                float: left;
                margin: 5px;
            }
        </style>
        <script src="../jquery.min.js"></script>
    </head>
    <body>
        <p><button id="go">Run »</button></p>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <div class="block"></div>
        <script>
            $(function() {
                $("#go").click(function() {
                    $(".block:first").animate({
                        left: 100
                    }, {
                        duration: 1000,
                        step: function(now, fx) {
                            $(".block:gt(0)").css("left", now);
                        }
                    });
                });
            })
        </script>
    </body>
</html>
```

### 11.stop()：停止指定元素的动画效果。

slideToggle()动画的效果是切换卷帘门，当鼠标进入或者离开的时候会触发切换效果，该动画默认有一定的执行时间，只有当时间间隔到达时才会停止动画，并不是说鼠标离开就会停止动画，利用stop()方法可以立即停止动画，没有时间间隔，这样的效果更贴近现实情况一些。

```javascript
 <script type="text/javascript">
  $(document).ready(function(){
                $(".wrap > ul > li").hover(function(){
                     $(this).children("ul").stop().slideToggle();
                })
            })
</script>
```
我们都知道队列的特点是先进先出，stop()函数中也存在着队列的特点。

```javascript
//参数一：是否清空动画队列
//参数二：是否立即执行完当前动画，没有时间间隔。

$("div").stop(false,true); //$("div")表示要停止动画的元素

用法及解释：

stop(true,true)：清空队列，并且当前动画不在缓慢执行而是直接完成。

stop(false,true):不清空队列，并且当前元素不再缓慢执行而是直接完成。

stop(true)：清空队列，停止当前动画

stop():不清空队列，停止当前动画，继续执行下一个动画
```

举例：队列的特点：先进先出,也就是说先进来的执行

```javascript

<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            div {
                width: 50px;
                height: 50px;
                background-color: red;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js">
        </script>
        <script type="text/javascript">
            $(document).ready(function() {
                $("input:eq(0)").click(function() {
                    $("div").animate({
                        width: "100%"
                    }, 3000);
                    $("div").animate({
                        height: "300px"
                    }, 3000);

                    $("div").animate({
                        width: "60%"
                    }, 3000);

                    $("div").animate({
                        height: "50px"
                    }, 3000);
                })

                $("input:eq(1)").click(function() {
                    $("div").stop();
                })
                $("input:eq(2)").click(function() {
                    $("div").stop(true,false);
                })
                $("input:eq(3)").click(function() {
                    $("div").stop(true, true);  //不仅清空队列，而且立即执行完成
                })
                $("input:eq(4)").click(function() {
                    $("div").stop(false, true);
                })
            })
        </script>
    </head>
    <body>
        <div></div>
        <input type="button" value="按钮" />
        <input type="button" value="stop1" />
        <input type="button" value="stop2" />
        <input type="button" value="stop3" />
        <input type="button" value="stop4" />
    </body>
</html>
```

大家看下这个例子，自己可以每个试一下，就能很清晰的明白了这些参数之间的区别。

jQuery动画总结：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232448/o_1.png)


