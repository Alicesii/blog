---
title: JS封装库
comments: true
date: 2018-05-28 15:48:54
categories:
tags: Yquery
about:

---

> 如果了解并且用过JQuery的人，应该知道它的强大与方便吧，是不是也想自己利用原生的JavaScript封装一个简单的属于自己的库呢？

在JQuery的选择器中，有三种类型的参数，我们可以传一个函数、传一个字符串、传一个对象。如果传进来的是字符串的话，又分为三种类型，`$("#ID")`,`$(".class")`,`$("selector")`

```javascript
$(function(){
    })==>事件绑定

$("sEv")=$("#ID"),$(".class"),$("selector")

$(obj)==>直接插入
```
### 1.第一步：封装函数

```javascript
function Yquery(yArg){
    //yArg这里表示任何类型的参数，命名要规范。
    //我们需要根据传进来的不同参数做不同的事情
    //首先判断传进来的参数的类型
    switch (typeof yArg){
        //如果传进来的参数是函数类型,就直接让window.onload执行
        case 'function':
          window.onload=yArg;
            break;
    }
}
```

封装好了函数，是不是内心有一丝的小激动呢，那我们现在赶紧来测试一下吧。

```javascript
<script>
    new Yquery(function(){
          console.log("a")
     })
</script>

```
我们可以在控制台中看到a，这样一个简单的封装就好了，但是这里面存在这一个极大的问题。我们熟悉jQuery的人都知道可以在一个页面中写多个入口函数，并且不会发生冲突问题。

```javascript
$(document).ready(function){

}
$(document).ready(function){

}
$(document).ready(function){

}
```

如果我们在自己封装的库里面写多个呢?会出现什么情况呢？

```javascript
<script>
    new Yquery(function(){
        console.log("a")
    })
    new Yquery(function(){
        console.log("b")
    })
</script>
```

我想大家应该都知道结果了，结果为b,并没有a,这就说明函数b覆盖了函数a,原因是我们把函数直接交给window.onload去执行，大家应该都知道在JavaScript中只能有一个入口函数`"window.onload=function(){}"`如果有多个的话，就会产生覆盖。

### 2.第一次修改Yquery.js

如何解决函数覆盖的问题？

在Jquery中采用的是事件队列的机制，这里面使用事件绑定机制。我们需要定义一个绑定事件的函数，`myAddEvent()`;

```javascript
function myAddEvent(obj,sEv,fn){
    //IE浏览器
    if(obj.attachEvent){
        obj.attachEvent("on"+sEv,fn);
   //标准浏览器
    }else{
        obj.addEventListener(sEv,fn,false);
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
    }
}
function Yquery(yArg){
    //yArg这 里表示任何类型的参数，命名要规范。
    //我们需要根据传进来的不同参数做不同的事情
    //首先判断传进来的参数的类型
    switch (typeof yArg){
        //如果传进来的参数是函数类型,就直接让window.onload执行
        case 'function':
          //使用绑定的方式，
          myAddEvent(window,"load",yArg);
             break;
    }
}
```

### 3.封装字符串Yquery.js

描述：

* 我们只需要判断第一个字符就可以判断传进来的字符串具体是哪一种类型(ID选择器、标签选择器、类选择器).

* 选择符本身只是用来选择元素的，而不对它做任何操作，对它做操作的是选择符后面的方法。

* `substring(1)`：截取字符串，从位置1开始截取，一直到结束，返回值是截取的字符串，在这里这个方法非常有用。

* 用一个参数保存选择器选择的元素，`this.elements = []`;

* `getByClass()`：类选择器的获取，`getElementsByTagName()`、`className()`;

```javascript
//通过JavaScript操作DOM方式获取类选择器
function getByClass(oParent, sClass) {
    var aEle = oParent.getElementsByTagName('*');
    var aResult = [];
    var i = 0;
   for(i = 0; i < aEle.length; i++) {
        if(aEle[i].className == sClass) {
            aResult.push(aEle[i]);
        }
    }
    return aResult;
}
function Yquery(yArg) {
    //yArg这 里表示任何类型的参数，命名要规范。 
    //我们需要根据传进来的不同参数做不同的事情
    //首先判断传进来的参数的类型
    this.elements = []; //用来保存选择器选择的元素
    switch(typeof yArg) {
            //如果传进来的参数是字符串类型，又分为三种情况
        case 'string':
            switch(yArg.charAt(0)) {
                case '#': //ID
                //如果选择器是ID选择器，那么yArg的值可能是"#div",我们需要把#去掉，剩下的才是我们想要的
                var obj = document.getElementById(yArg.substring(1));
                //这时，elements数组就存了被ID选择器选中的字符串
                this.elements.push(obj);
                break;
                case '.': //class
                //从哪个父元素选择元素？从整个文档选取类选择器。
                //用substring(1)去掉类选择器的". "
                //用返回的数组aResult，替换elements数组
                this.elements = getByClass(document, yArg.substring(1));
                break;
                default: //tagName
                    this.elements = document.getElementsByTagName(yArg);
            }
            break;
    }
}
```

字符串的封装就完成了，现在我们来测试一下吧。

```javascript
    <body>
        <button>我是按钮</button>
    </body>
    <script>
         new Yquery(function(){
                    new Yquery("button").click(function(){
                        console.log("a")
                    })
                })
        </script>
```

输出结果：在控制台中可以看到a,这里我只测试了标签选择器，大家也可以测测ID选择器和类选择器.

### 4.封装对象Yquery.js

封装对象特别简单，只需要把对象直接`push`到`elements`数即可。

```
function Yquery(yArg) {
    //yArg这 里表示任何类型的参数，命名要规范。 
    //我们需要根据传进来的不同参数做不同的事情
    //首先判断传进来的参数的类型
    this.elements = []; //用来保存选择器选择的元素
    switch(typeof yArg) {
        case 'object':
        //如果传进来的参数是字符串对象类型，就直接放到elemens数组
               this.elements.push(yArg);
    }
}
```

### 5.入口函数的封装

我们现在自己封装的库每次测试的时候的入口函数为

```javascript
<script>
    new Yquery(function(){
        })
</script>
```

我们参照一下JQuery的入口函数

```javascript
<script>
    $(document).ready(function(){

        })

   $(function(){

    })
</script>
```
那我们怎样才能实现这样的写法呢？

做法：声明一个`$`符号函数，函数内部返回我们的构造函数，以代替每次的的入口函数都要写`new Yquery()`

```javascript

//声明一个$符号函数，函数内部返回我们的构造函数以代替每次的的入口函数都要写new Yquery

function $(yArg){
    return new Yquery(yArg);
}
```
经过这样修改之后，我们的入口函数就可以写成下面的形式了。

```javascript
<script>
    $(document).ready(function(){

        })

   $(function(){

    })
</script>
```

### 6.封装类似JQuery中的`click()`函数

```javascript
//给Yquery函数绑定click事件
Yquery.prototype.click = function(fn) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加单击事件
    var i = 0;
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        myAddEvent(this.elements[i], "click", fn);
    }
};
```

### 7.封装类似JQuery中的`show()`函数和`hide()`函数

`display:block==show()`

`display:none==hide()`

```javascript
//封装类似jQuery中的show()函数

Yquery.prototype.show= function() {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加show事件
    var i = 0;
    //elements数组里面放的就是被选中的元素,让被选中的每一个元素都在展示出来
    for(i = 0; i < this.elements.length; i++) {
        this.elements[i].style.display="block";
    }
};

//封装类似jQuery中的hide()函数
Yquery.prototype.hide= function(fn) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加show事件
    var i = 0;
    //elements数组里面放的就是被选中的元素,让被选中的每一个元素都在隐藏起来
    for(i = 0; i < this.elements.length; i++) {
        this.elements[i].style.display="none";
    }
};
```

### 8.封装`hover()`函数

```javascript
//封装类似jQuery中的hover()函数
//hover()方法需要两个参数，一个是mouseenter(),一个是mouseleave()
Yquery.prototype.hover= function(fnEnter,fnLeave) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加hover方法
    var i = 0;
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        myAddEvent(this.elements[i], "mouseenter", fnEnter);
        myAddEvent(this.elements[i], "mouseleave", fnLeave);
    }
};
```

那我们现在来测试一下`hover()`函数

```javascript
<body>
   <div id="div1" style="width: 100px; height: 100px; background: red;">
   </div>
</body>
<script>
    $(function() {
        $('#div1').hover(function (){
            document.title='mouseenter';
         }, function (){
            document.title='mouseleave';
      });
</script>
```

### 9.封装`css()`函数

参数个数不固定(arguements.length)

注意：在利用css()函数获取值的时候，如下代码

`$("div").css("background-color");`

,假设现在页面上有三个div，到底获取的是哪一个"div"的背景颜色值呢？

答案：是获取第一个匹配元素的样式属性值。

那我们这样封装`css()`函数有问题吗？

```javascript
//封装css(方法)
//CSS()方法主要有两个功能：设置样式(两个参数)和获取样式(一个参数),也就是说参数个数不固定
Yquery.prototype.css = function(attr, value) {
    if(arguments.lenght == 2) //设置样式
    {
        //不同的选择器可能选择多个元素，给每个选中的元素都需要添加css方法
        var i = 0;
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++) {
            this.elements[i].style[attr] = value;
        }

    } else {   //获取样式

        //获取第一个匹配元素的样式属性值,直接给style()方法
        return this.elements[0].style[attr];
    }
}
```

肯定是有问题的，Javascript中的`style()`方法在设置属性值的时候一切正常，但是在获取值的时候却出现了问题：Javascript中的`style()`方法只能获取的是行间样式，其他的样式值获取不到。但是绝大样式都不是行间样式，都是写在一个外部文件然后引入。

什么是行间样式呢？就是直接写在标签元素内部的,例如下面的代码。

```javascript
<div style="width:150px;height:150px;background-color:pink;"></div>
```
如果不相信的话我们可以测试一下

```javascript
<style>
   #div1 {
      width: 100px;
      height: 100px;
      background: red;
      }
</style>
    <body>
        <div id="div1">
        </div>
    </body>
    <script>
            $(function() {
                $('#div1').click(function() {
                    console.log($('#div1').css('width'));
                });
            });
        </script>
```

这样的形式获取的width值为空，那我们换成行间样式在测试一下，

```javascript
 <body>
        <div id="div1" style="width:150px;height:150px;background-color:pink;">
        </div>
    </body>
    <script>
            $(function() {
                $('#div1').click(function() {
                    console.log($('#div1').css('width'));
                });
            });
        </script>
```

这样的形式就可以获取到CSS的width值。

我们总不可能以后把样式就写在标签内部吧，我们总得寻找一个解决的办法。

解决办法：我们封装一个`getStyle()`函数，然后绑定在`CSS()`函数的获取值的方法中。

用到的方法：`currentStyle`属性、`getComputedStyle()`方法：

```javascript>
//getStyle()：用来获取CSS样式属性值。
function getStyle(obj,attr){
    if(obj.currentStyle)
    {
        return obj.currentStyle[attr];
    }
    else{
        return getComputedStyle(obj,false)[attr];

    //参数中的false并没有什么实际的意义，只要随便传一个值就行，
    //你也可以写true,或者随便的一个数字、字符。
    }
}
//封装css(方法)
//CSS()方法主要有两个功能：设置样式(两个参数)和获取样式(一个参数),也就是说参数个数不固定
Yquery.prototype.css = function(attr, value) {
    if(arguments.length == 2) //设置样式
    {
        //不同的选择器可能选择多个元素，给每个选中的元素都需要添加css方法
        var i = 0;
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++) {
            this.elements[i].style[attr] = value;
        }
    } else {   //获取样式
        //获取第一个匹配元素的样式属性值。
        return getStyle(this.elements[0],attr);
    }
}
```

现在的代码应该就没有什么问题了，我们可以测试一下发现，不论是行间样式的属性值，还是外部引入的，我们都可以获得它的属性值，而不会是一个空值。

### 10.第二次修改关于this

在测试中，我们总能发现各种各样有趣的问题，现在再来看一个有趣的问题吧。

```javascript
<style>
            #div1 {
                width: 100px;
                height: 100px;
                background: red;
            }
        </style>
   <body>
        <div id="div1">
        </div>
    </body>
 <script>
            $(function() {
                $('#div1').hover(function (){
                    $('#div1').css('background-color', 'green');
                }, function (){
                   $('#div1').css('background-color', 'red');
                });
            });
        </script>
```

我们可以发现上述代码运行的结果本身并没有什么问题，但是在代码书写的方面确是有些怪异的。
如果上述代码我们引入jQuery的库，我们肯定是这样写的:

```javascript
<style>
            #div1 {
                width: 100px;
                height: 100px;
                background: red;
            }
        </style>
   <body>
        <div id="div1">
        </div>
    </body>
 <script>
            $(function() {
                $('#div1').hover(function (){
                    $(this).css('background-color', 'green');
                }, function (){
                   $('this').css('background-color', 'red');
                });
            });
        </script>
```
直接用this替换？？我们自己写的库可以做到吗？我们来测试一下

我们可以发现在标准的浏览器下，换成this这个也是可以顺利执行的，但是在IE浏览器下不出乎意料的就报错了，IE浏览器总是让我们头疼的浏览器,我们来想一下这是为什么呢?

我们`console.log`一下this,看下标准浏览器和IE浏览器的差异。

```javascript
$(function (){
    $('#div1').hover(function (){
          console.log(this);
          alert(this);
        $(this).css('background', 'green');
    }, function (){
        $(this).css('background', 'red');
    });
});
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_1.png)

* 谷歌浏览器下this指向这个对象div本身.

* 比较老的IE浏览器下的this指向的是window，this指向window而不是指向对象div本身，肯定就不出乎意料的就报错了。

这时我们就想起来了不能用this的四种情况：

**行间样式、套一层、绑定、定时器。**

`hover()`函数是把`mouseenter()`和`mouseleave()`通过绑定封装的，我们再看一下我们的绑定函数

```javascript
function myAddEvent(obj,sEv,fn){
    //IE浏览器
    if(obj.attachEvent){
        obj.attachEvent("on"+sEv,fn);
   //标准浏览器
    }else{
        obj.addEventListener(sEv,fn,false);
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
    }
}
```
IE下面的绑定`attachEvent()`有bug,会把被绑的函数中的this指向window,而不是指向对象本身,标准浏览器下面的`addEventListener()`没有这个问题，this就是指向对象本身，

那我们怎么去解决IE浏览器下的这个问题呢？为了统一，我们把它们看作是都有bug的，这样处理起来比较方便。我们可以利用`call()`函数和`apply()`函数，改变this的指向。所以我们改变一下绑定函数`myAddEvent()`。

```javascript
function myAddEvent(obj,sEv,fn){
    //IE浏览器
    if(obj.attachEvent){
        obj.attachEvent("on"+sEv,function(){
            fn.call(obj);
        });
   //标准浏览器
    }else{
        obj.addEventListener(sEv,fn,false);
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
    }
}
```
通过`call()`函数我们手动将IE浏览器的this指向了对象，这下就和标准浏览器执行一样的效果了。

### 11.toggle()函数的封装

`toggle()`函数有两个参数，这两个参数可以来回切换：

参照`toggle()`函数我们可以发现一个规律：如果我们给一个按钮添加一个click事件，第一次点击按钮的时候，触发fn1函数，第二次点击按钮的时候触发fn2函数，第三次点击按钮的时候触发fn1函数，第四次点击按钮的时候触发fn2函数，以此类推，所以，我们需要设置一计数器。

#### 11.1.计数器的封装

```javascript
<body>
<input id="btn1" type="button" value="我是按钮" />
</body>
<script>
window.onload=function ()
{
    var oBtn=document.getElementById('btn1');
    var count=0;
    oBtn.onclick=function ()
    {
        console.log(count++);
    };
};
</script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_2.png)

上述形式的计数器在只有一个button按钮,没有一点问题，那如果页面上存在多个Button按钮、并且想要这些按钮都可以计数，而且是分别从0开始计数，这个该怎么实现呢？

```javascript
<body>
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />
<input type="button" value="按钮4" />
</body>
<script>
window.onload=function ()
{
    var aBtn=document.getElementsByTagName('input');
    var i=0;
    var count=0;
    
    for(i=0;i<aBtn.length;i++)
    {
        aBtn[i].onclick=function ()
        {
            console.log(count++);
        };
    }
};
</script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_3.png)

我们打开控制台测试了一下发现：这些按钮并不是分别从0开始计数的，而是累加起来计数的，那我们怎么样可以分离每个按钮的计数呢？让每个按钮都分别从0开始计数，那我们可能需要多个计数器。这样才可以分离每个按钮的计数情况。

解决办法：我们封装一个`addClick()`方法，我们先来简单的封装一下。

```javascript
<body>
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />
<input type="button" value="按钮4" />
</body>
window.onload=function ()
{
    var aBtn=document.getElementsByTagName('input');
    var i=0;
    var count=0;
    for(i=0;i<aBtn.length;i++)
    {
        addClick(aBtn[i]);
    }
    function addClick(obj)
    {
        obj.onclick=function ()
        {
             console.log(count++);
        };
    }
};
</script>
```
这样封装其实并没有什么作用，运行结果还是如上图所示，这些按钮并不是分别从0开始计数的，而是累加起来计数的。

我们可以再来稍微改变一下。我们目前最需要的就是增加多个计数器，我们可以这样做。

这里用的其实是闭包的特性。

```javascript
<body>
<input type="button" value="按钮1" />
<input type="button" value="按钮2" />
<input type="button" value="按钮3" />
<input type="button" value="按钮4" />
</body>
<script>
    window.onload = function() {
        var aBtn = document.getElementsByTagName('input');
        var i = 0;
        for(i = 0; i < aBtn.length; i++) {
            addClick(aBtn[i]);
        }
        function addClick(obj) {
            var count = 0;
            obj.onclick = function() {
                 console.log(count++);
            };
        }
    };
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_4.png)

经过这么一番改变，我们想要的的效果可算是好了，不同的按钮会从0开始计数，有多少个按钮就会添加多少个计数器。

我们可以改变一下函数的形式，让它变得更加好看一些，更加符合JS的语法规范,熟悉JavaScript语法的人都知道在JavaScript中函数有两种形式，一种是带参数的形式，一种是不带参数的形式，那我们可以这样改写一下。

```javascript

function a(){
    console.log(a);
}
div.onclick=a;

div.onclick=function(){
    console.log(a);
}

//上面两种形式的函数的写法都是正确的。
```

那参照上面我们可以将我们的计数器函数也改变一下。

`(function(){})()`

```javascript
window.onload=function ()
{
    var aBtn=document.getElementsByTagName('input');
    var i=0;
    for(i=0;i<aBtn.length;i++)
    {
        (function (obj){
            var count=0;
            obj.onclick=function ()
            {
                alert(count++);
            };
        })(aBtn[i]);
    }
};
</script>
```

我们对于这样的写法可能还不太满意，那么我们可以再来改变一下,一个更加简便的方法。

```javascript
<script>
window.onload=function ()
{
    var aBtn=document.getElementsByTagName('input');
    var i=0;
    for(i=0;i<aBtn.length;i++)
    {
        aBtn[i].onclick=(function (count){
            return function (){
                alert(count++);
            };
        })(0);
    }
};
</script>
```

### 11.2我们接着来封装我们toggle()函数

```javascript
//封装toggle()函数
//toggle()函数可以有无数多个参数
Yquery.prototype.toggle = function() {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加toggle方法
    var i = 0;
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        //计数函数的封装
        (function addToggle(obj) {
            var count = 0;
            myAddEvent(obj, 'click', function() {
                //arguments()函数获取一个函数形参的个数
                //这里面的函数指的是就是封装的toggle()函数，
                //也就是说，arguments()函数获取的是toggle()函数的形参的个数。
                arguments[count % arguments.length].call(obj);
                count++;
            })
        })(this.elements[i]);
    }
}
```
这样封装对不对呢？我们来测试一下。

```javascript
<script>
$(function (){
    $('input').toggle(function (){
        console.log('a');
    }, function (){
        console.log('b');
    }, function (){
       console.log('c');
    });
});
</script>
<body>
<input type="button" value="1" />
<input type="button" value="2" />
<input type="button" value="3" />
<input type="button" value="4" />
</body>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_5.png)
经过测试，我们可以发现报错了，那原因是为什么呢？

我们给myAddEvent()中`console.log(arguments.length);`发现值为1，并且还报错了，可是我们明明给`toggle()`函数传了3个函数，按道理说值应该为3呀，

通过以往我们学javascript的经验，我们可以知道在javascript中一个this、另一个就是arguments非常容易混乱，这里就是arguments在捣鬼。我们再次测试一下，

```javascript
Yquery.prototype.toggle = function() {
    var i = 0;
   console.log(arguments.length);//在控制台输出3

    for(i = 0; i < this.elements.length; i++) {
        //计数函数的封装
        addToggle(this.elements[i]);
    }
    function addToggle(obj) {
        var count = 0;
        myAddEvent(obj, 'click', function() {
            console.log(arguments.length);//在控制台输出1，并且报错
            arguments[count%arguments.length].call(obj);
            count++;
        })
    }
}
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_6.png)

这下我们就明白了，我们需要保存输出结果为3的这个`arguments.length`的值，用这个值去绑定我们的`myAddEvent()`函数肯定就是不会错了。如下代码

### 11.3.第三次修改toggle()函数

```javascript
//封装toggle()函数
//toggle()函数可以有无数多个参数
Yquery.prototype.toggle = function() {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加toggle方法
    var i = 0;
    //我们需要提前将arguments的值保存下来，以免被后面的函数的arguments所覆盖。
    var _arguments=arguments;
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        //计数函数的封装
        addToggle(this.elements[i]);
    }
    function addToggle(obj) {
        var count = 0;
        myAddEvent(obj, 'click', function() {
            //arguments()函数获取一个函数形参的个数
            //这里面的函数指的是就是封装的toggle()函数，
            //也就是说，arguments()函数获取的是toggle()函数的形参的个数。
            _arguments[count% _arguments.length].call(obj);
            count++;
        })
    }
}
```
我们可以再来测试一下

```javascript
<script>
$(function (){
    $('input').toggle(function (){
        console.log('a');
    }, function (){
        console.log('b');
    }, function (){
       console.log('c');
    });
});
</script>
<body>
<input type="button" value="1" />
<input type="button" value="2" />
<input type="button" value="3" />
<input type="button" value="4" />
</body>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_7.png)

这下就完全没有问题了

### 11.4.封装的toogle()函数的小实例

```javascript
<style>
#div1 {width:100px; height:100px; background:red;}
</style>
<script>
$(function (){
    $('input').toggle(function (){
        $('#div1').hide();
    }, function (){
        $('#div1').show();
    });
});
</script>
<body>
<input type="button" value="我是按钮" />
<div id="div1">
</div>
</body>
```
点击按钮会隐藏，如果在点击一次按钮会显示，这就是`toogle()`函数的效果。

12.封装`attr()`方法和封装`css()`方法有些类似

```javascript
//封装attr(方法)
//attr()方法主要有两个功能：设置属性(两个参数)和获取属性(一个参数),也就是说参数个数不固定
Yquery.prototype.attr = function(attr, value) {
    if(arguments.length == 2) //设置属性值
    {
        //不同的选择器可能选择多个元素，给每个选中的元素都需要添加attr方法
        var i = 0;
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++) {
            this.elements[i][attr] = value;
        }
    } else { //获取属性值
        //获取第一个匹配元素的属性值。
        return this.elements[0][attr];
    }
}
```
我们可以简单地测试一下：

```javascript
<body>
<input id="txt1" type="text" />
<input id="btn1" type="button" value="读取文字" />
<input id="btn2" type="button" value="设置文字" />
</body>
<script>
$(function (){
    $('#btn2').click(function (){
        $('#txt1').attr('value', '我是设置的值')
    });
    $('#btn1').click(function (){
        console.log($('#txt1').attr('value'));
    });
});
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_8.png)

### 13.封装eq()方法，索引值从0开始。

```javascript
//封装eq()函数
//eq()函数有一个参数，获取第几个元素，索引值从0开始。
Yquery.prototype.eq=function (n){
  //所有能选择到的元素都存在elements数组中
  //需要将返回的普通DOM对象封装成我们自己写的Yquery对象。
  return $(this.elements[n]);
}
```
我们可以测试一下

```javascript
<style>
    div {
          width: 100px;
          height: 100px;
          background: red;
          float: left;
          margin: 10px;
        }
</style>
    <body>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </body>
<script>
    $(function() {
        $('div').eq(0).css('background', 'green');
    });
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_9.png)

### 14.封装find()方法

在jQuery的`find()`方法中的作用是查找指定元素的子元素。

`find()`方法的参数只有一个，但是都有什么类型的呢？我们可以先看一下在jQuery库中find()方法都有哪些参数？

```javascript
<body>
        <ul id="ul1">
            <li></li>
            <li></li>
            <li></li>
        </ul>
        <ul>
            <li class="box"></li>
            <li class="box"></li>
            <li></li>
        </ul>
        <ol>
            <li></li>
            <li></li>
            <li></li>
        </ol>
    </body>
    $("ul").find("li");
    $("ul").find("ol");
    $("#ul1").find("li");
    $("ul").find(".box");

    //注意find()方法没有这种形式：
    $("#div1").find("#ul1");
```
由此我们可以得出`find()`方法的参数有两种形式：一种直接是标签名的形式、一种是类名的形式。

```
//封装find()函数
//find()函数只有一个参数，但是参数分为两种形式
//一种直接是标签名的形式、一种是类名的形式。
Yquery.prototype.find = function(str) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加find()
    var i = 0;
    var aResult = []; //用来存放临时数据的数组
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        switch(str.charAt(0)) {
            case '.': //参数是类名的形式
                var aEle = getByClass(this.elements[i], str.substring(1));
                aResult = aResult.concat(aEle);
                break;
            default: //参数是标签名的形式
                var aEle = this.elements[i].getElementsByTagName(str);
                aResult = aResult.concat(aEle);
        }
    }
    return aResult;
}
```
我们现在已经算是初步封装好了，那有没有什么问题呢？？我们看一下源代码，我们直接返回的是一个数组aResult，我们可以直接返回一个普通的数组吗？？

答案：这是不可以的，我们一般情况下利用`find()`方法找到一个元素或者多个元素，是直接要给后面添加我们封装好的方法的,比如`css()、show()、click()`方法等，但是普通的数组没有这些方法，我们不可能强行的加上去，所以我们还是需要再改改的。

解决办法：我们需要创建一个空的Yquery对象`newYquery`，这个空的Yquery对象newYquery可以直接给后面添加我们封装好的方法的,比如`css()、show()、click()`方法等，也就是说，用一个空的Yquery对象newYquery来装载我们的普通的数组，把JavaScript对象转化成Yquery对象。

```javascript
//封装find()函数
//find()函数只有一个参数，但是参数分为两种形式
//一种直接是标签名的形式、一种是类名的形式。
Yquery.prototype.find = function(str) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加find()
    var i = 0;
    var aResult = []; //用来存放临时数据的数组
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        switch(str.charAt(0)) {
            case '.': //参数是类名的形式
                var aEle = getByClass(this.elements[i], str.substring(1));
                aResult = aResult.concat(aEle);
                break;
            default : //参数是标签名的形式
                var aEle = this.elements[i].getElementsByTagName(str);

                aResult = aResult.concat(aEle);
        }
    }
    var newYquery = $(); //我们需要创建一个空的Yquery对象，
    newYquery.elements = aResult;
    return newYquery;
};
```
那现在赶紧来测试一下吧。

```javascript
    <body>
        <ul id="ul1">
            <li>我是无序列表</li>
            <li>我是无序列表</li>
            <li>我是无序列表</li>
        </ul>
        <ul>
            <li class="box">我是box</li>
            <li class="box">我是box</li>
            <li>我是无序列表</li>
        </ul>
        <ol>
            <li>我是有序列表</li>
            <li>我是有序列表</li>
            <li>我是有序列表</li>
        </ol>
    </body>
    <script>
         $(function() {
                $('ul').find('.box').css('background-color', 'red');
                $('ul').find('li').css('background-color', 'red');
            });
    </script>
```

经过测试，我们可以发现，可以正确的找到参数是类名的形式并且正确的设置样式，但是不能找到参数是标签名的形式，而且会报错，原因是使用的`concat()`方法。那我们先来看下`concat()`方法。

concat()方法：用于连接两个或多个数组，该方法不会改变现有的数组,而仅仅会返回被连接数组的一个副本。

参数是类名的形式可以使用concat()方法的原因：`getByClass()`方法获取的值的返回结果就是一个数组。

参数是标签名的形式不能使用`concat()`方法的原因：通过`getElementsByTagName('li')`;获取到的是一个HTML集合，虽然看起来像是一个数组，但不是一个数组，并不具备数组的操作，所以不能用concat()方法来连接。

```javascript
<script type="text/javascript">
      window.onload=function(){
        var li=document.getElementsByTagName('li');
             console.log(li)
    }
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_10.png)

我们可以再看一下数组的集合。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_11.png)

解决办法：我们封装一个函数`appendArray()`实现连接两个或两个以上HTML集合的功能。

```javascript
//函数appendArray():实现连接两个或两个以上HTML集合的功能
function addendArr(arr1, arr2) {
    var i = 0;
    //把arr2数组的值都push到arr1中。
    for(i = 0; i < arr2.length; i++) {
        arr1.push(arr2[i]);
    }
}
```
这下可以看一下我们终极的`find()`方法。

```javascript
//封装find()函数
//find()函数只有一个参数，但是参数分为两种形式
//一种直接是标签名的形式、一种是类名的形式。
Yquery.prototype.find = function(str) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加find()
    var i = 0;
    var aResult = []; //用来存放临时数据的数组
    //elements数组里面放的就是被选中的元素
    for(i = 0; i < this.elements.length; i++) {
        switch(str.charAt(0)) {
            case '.': //参数是类名的形式
                var aEle = getByClass(this.elements[i], str.substring(1));

                aResult = aResult.concat(aEle);
                break;
            default: //参数是标签名的形式
                var aEle = this.elements[i].getElementsByTagName(str);
 
                addendArr(aResult, aEle);
        }
    }
    var newYquery = $(); //我们需要创建一个空的Yquery对象，
    newYquery.elements = aResult;
    return newYquery;
};
```
我们再来测试一下上面有问题的代码，这下可以发现不论是参数是标签形式的，还是参数是类形式的都可以正确的获取到指定元素，并且正确的设置样式。

### 15.封装index()方法

描述：

* 从匹配的元素中搜索给定元素的索引值，从0开始计数。

* 获取元素的索引值(同辈元素)，索引的序号从0开始。

* 这时的this.elements会有好多个元素，因为elements数组里面放的就是被选中的元素，

* 和封装css()方法，如果css()只有一个参数(获取属性值)时类似,如果有多个匹配的元素，只获取第一个匹配元素的属性值。this.elements[0]

* 我们现在最主要的就是获取指定元素在其同辈元素的位置，

我们可以先看一个小案例，点击不同的按钮然后返回对应的索引值

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>无标题文档</title>
        <script>
            function getIndex(obj) {
                //获取obj的父节点(body)的孩子节点(4个<input>标签)
                var aBrother = obj.parentNode.children;
                var i = 0;
                //obj的节点肯定和aBrother的某个节点相等，可以循环遍历判断一下
                for(i = 0; i < aBrother.length; i++) {
                    if(aBrother[i] == obj) {
                        return i;
                    }
                }
            }
            window.onload = function() {
                var aBtn = document.getElementsByTagName('input');
                var i = 0;
                for(i = 0; i < aBtn.length; i++) {
                    aBtn[i].onclick = function() {
                        console.log(getIndex(this));
                    };
                }
            };
        </script>
    </head>
    <body>
        <input type="button" value="我是按钮1" />
        <input type="button" value="我是按钮2" />
        <input type="button" value="我是按钮3" />
        <input type="button" value="我是按钮4" />
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_12.png)

这下我们想要的效果达成了，我们可以将上面的getIndex()方法应用到我们的案例上面。

封装的index()方法：

```javascript

//获取指定元素在同辈元素处的索引值。
function getIndex(obj) {
    //获取obj的父节点(body)的孩子节点(4个<input>标签)
    var aBrother = obj.parentNode.children;
    var i = 0;
    //obj的节点肯定和aBrother的某个节点相等，可以循环遍历判断一下
    for(i = 0; i < aBrother.length; i++) {
        if(aBrother[i] == obj) {
            return i;
        }
    }
}
//封装index()方法
Yquery.prototype.index = function() {
    return getIndex(this.elements[0]);
}
```
还是来测试一下吧，是不是有点不放心写的对不对？？

```javascript
    <body>
        <button>我是按钮1</button>
        <button>我是按钮2</button>
        <button>我是按钮3</button>
        <button>我是按钮4</button>
        <button>我是按钮5</button>
    </body>
   <script type="text/javascript">
            $(function(){
                $("button").click(function(){
                    var index=$(this).index();
                    console.log(index);
                })
            })
        </script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_13.png)

看到这样的结果就放心了。

我们现在封装了这么多的方法，那我们现在做一个小案例吧，来满足一个我们的成就感。

### 16.高级版的css()方法的封装

在我们以前封装的css()方法中，可以设置或者获取属性，但是我们封装的时候，有两个参数的话是设置属性值，有一个参数的话是获取属性值，所以局限于获取或者设置一个属性值，例如：

```javascript
.css('background-color','#ffe')//设置背景色

.css('background-color')//获取背景色
```
那如果想要一次添加多个属性值，或者通过链接的结构设置属性值，例如下面的样式，该怎么实现呢？

```
.css({width: '200px', height: '200px', background:'red'})

.css(' width', '200px'').css('height','200px').css('background','green')
//把添加多个属性值的形式看作是JSON字符串
var obj={width: '200px', height: '200px', background:'red'}
.css(obj)
```
我们可以把一次性添加多个属性值的这种看作是一个json字符串，那么在css()方法中当有一个参数时不仅能表示获取属性值了，而且还可以表示设置多个属性值。

那么我们怎么循环遍历json字符串呢？利用for..in循环

```javascript
<script>
var json={width: '200px', height: '200px', background:'red'};
var i='';
for(i in json)
{
   console.log(i+':'+json[i]);
}
</script>
```
输出结果

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_16.png)

```javascript
//封装css(方法)
//CSS()方法主要有两个功能：设置样式(两个参数)和获取样式(一个参数),也就是说参数个数不固定
Yquery.prototype.css = function(attr, value) {
    if(arguments.length == 2) //设置样式
    {
        //不同的选择器可能选择多个元素，给每个选中的元素都需要添加css方法
        var i = 0;
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++) {
            this.elements[i].style[attr] = value;
        }
    } else { //可能是设置多个样式也可能是获取样式
        if(typeof attr=='string'){
        //如果是字符串类型的话还是获取样式属性值。
        //获取第一个匹配元素的样式属性值。
        return getStyle(this.elements[0], attr);
     }else{//表示是一个json形式的字符串，设置多个样式属性值
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++)
        {//现在开始循环attr,JSON字符串
            var k='';  //空字符串
            for(k in attr){
                this.elements[i].style[k]=attr[k];
            }
        }
     }
  }
};
```
高级版的css()方法我们也封装完了，我们可以来测试一下，

```javascript
<body>
    <input type="button" value="我是按钮" />
    <div id="div1">
    </div>
</body>
<script>
    $(function() {
        $('input').click(function() {
            $('#div1').css({
                width: '200px',
                height: '200px',
                background: 'green'
            });
        });
    });
</script>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_14.png)

现在我们实现了一次性添加多个属性值的效果,那链式操作的行为可以不可以实现呢？

`.css('width', '200px').css('height','200px').css('background','green');`

运行结果图17，可以发现只设置成功了第一个css的属性值，其他的并没有设置成功，并没有链式操作的属性，那么我们必须使我们封装的函数有链式操作的特点，那就必须在函数的结束部分加上`return this`.

最终极的css()函数的封装：

```javascript
//封装css(方法)
//CSS()方法主要有两个功能：设置样式(两个参数)和获取样式(一个参数),也就是说参数个数不固定
Yquery.prototype.css = function(attr, value) {
    if(arguments.length == 2) //设置样式
    {
        //不同的选择器可能选择多个元素，给每个选中的元素都需要添加css方法
        var i = 0;
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++) {
            this.elements[i].style[attr] = value;
        }
    } else { //可能是设置多个样式也可能是获取样式
        if(typeof attr=='string'){
        //如果是字符串类型的话还是获取样式属性值。
        //获取第一个匹配元素的样式属性值。
        return getStyle(this.elements[0], attr);
     }else{//表示是一个json形式的字符串，设置多个样式属性值
        //elements数组里面放的就是被选中的元素
        for(i = 0; i < this.elements.length; i++)
        {//现在开始循环attr,JSON字符串
            var k='';  //空字符串
            for(k in attr){
                this.elements[i].style[k]=attr[k];
            }
        }
     }
  }
  return this;
};
```
先不管是什么原因，我们先来测试一下。

```javascript
<script>
  $(function() {
      $('input').click(function() {
            $('#div1')
                .css('width', '200px')
                .css('height','200px')
                .css('background','green');
         });
    });
</script>
```

运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_14.png)

这下我们成功的封装了一次设置多个样式属性和链式操作。

大家是不是有个疑问？？为什么要`return this`呢？？

参考：jQuery常用方法之事件对象

我们先看一个小案例，然后在解释：

```javascript
        <script>
            function show(str) {
                console.log(str);

                return show;
            }
            show('abc')('bcd')(13)(444)('bdsf')(3123);
        </script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_15.png)

为什么函数可以一直一直调用呢？

这就是函数的链式操作，简单来说就是函数调用之后不断地返回自己，调用之后就返回自己，不断地往后前进。我们给函数的最后加上了return this,就可以实现Yquery库中的类似的链式操作。

我们可以给我们以前封装的函数都加上`return this`，它们就都有了链式操作的属性，可以随意的组合，具有强大的效果。

我们可以测试一下，看下这些方法是不是都可以随意地组合。

```javascript
<!DOCTYPE html >
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>无标题文档</title>
<script src="../Yquery.js"></script>
<script>
$(function (){
    $('input').hover(function (){
        document.title='mouseenter';
    }, function (){
        document.title='mouseleave';
    }).css({
        width: '200px', 
        height: '100px', 
        background: 'green'})
     .click(function (){
        console.log('a');
    });
});
</script>
</head>
<body>
<input type="button" value="我是按钮" />
</body>
</html>
```
图18：我们可以发现所有的混搭都可以实现，这下心里是不是很高兴呢？

### 17.JQuery中阻止默认事件

如果想要在jQuery中阻止默认事件，直接在它的事件处理函数中加上`return false`即可。

举例：`contextmenu()`:右键菜单

功能：禁止浏览器右键菜单的弹出。

```javascript
<script>
$(function (){
    $(document).on('contextmenu', function (){
        return false;
    });
});
</script>
```
### 18.JQuery中阻止冒泡

```javascript
<body>
<div style="background:red;">
    <div style="background:green;">
        <div style="background:#CCC;"></div>
    </div>
</div>
</body>
<script>
$(function (){
    $('div').click(function (){
        console.log(this.style.background);
        return false;
    });
});
</script>
```
描述：

* 没有阻止冒泡的输出(没有return false)：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_19.png)

* 阻止了冒泡的输出(有return false)：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_20.png)

注意：

* 在jQuery中，其实return false既代表了阻止默认事件，又代表了阻止冒泡。

* 但是在原生的javascript中，`return false`只代表阻止默认事件。

* 在我们自己封装的Yquery中，我们同样可以使用`return false`阻止默认事件和冒泡吗？

上述的同样的阻止默认事件和阻止冒泡的代码中，换成我们自己封装的Yquery库,测试一下，我们可以得出的结论是：

我们自己封装的Yquery库用`return false`当然不能阻止冒泡事件和阻止默认事件，原因是因为在JQuery阻止默认事件时，我们用`on()`方法绑定了`contextmenu`，但是在我们的Yquery库中却没有这个方法，所以我们也要封装一个`on()`方法，不能阻止冒泡事件的原因我们在后面再说，让我们先开始封装on()函数吧。

### 19.on()函数的封装

我们先暂时设定on()函数有两个参数，一个是事件名，一个是事件函数。

```javascript
//封装on()函数
Yquery.prototype.on=function(sEv,fn){
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加on()方法
    var i=0;
    //循环的给每个元素都绑定事件
    for(i=0;i<this.elements.length;i++)
    {
         //给每个元素绑定sEv事件，执行fn函数
        myAddEvent(this.elements[i],sEv,fn);
    }
}
```
我们来测试一下，看看有没有用

```javascript
<script>
$(function (){
    $(document).on('contextmenu', function (){
        return false;
    });
});
</script>
```

给过测试发现并没有什么用，根本就阻止不了默认行为，到底是哪里出了问题呢？再看一下之前封装的on()函数，先来排查错误，肯定不会是for()循环出错了，那么很有可能就是`myAddEvent()`函数有些问题，那我们再来看一下这个函数：

```javascript
function myAddEvent(obj, sEv, fn) {
    //IE浏览器
    if(obj.attachEvent) {
        obj.attachEvent("on" + sEv, function() {
            fn.call(obj);
        });
        //标准浏览器
    } else {
        obj.addEventListener(sEv, fn, false);
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
    }
}
```
myAddEvent()函数中的第三个函数fn其实就是我们传给`on()`函数的第二个参数，也就是事件处理函数，

```javascript
<script>
$(function (){
    $(document).on('contextmenu', function (){
        return false;
    });
});
</script>
```

事件处理函数中的return false，我们想一想有没有别的函数接收，其实根本就没有人接收，不管我们return什么值都没有其他的函数接收，return 什么都没有什么意思，因为根本就没有人接收，没人管你return什么值。所以我们需要修改myAddEvent()函数，给它里面添加return false。

我们先把IE浏览器的阻止默认事件封装好

```javascript
function myAddEvent(obj, sEv, fn) {
    //IE浏览器
    if(obj.attachEvent) {
        obj.attachEvent("on" + sEv, function() {
            if(false==fn.call(obj)){
                return false;
            }
        });
        //标准浏览器
    } else {
        obj.addEventListener(sEv, fn, false);
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
    }
}
```

在IE浏览器下测试，没有问题，阻止了右键的弹出事件。

那我们的标准浏览器该怎么办呀？

大家应该都知道如果事件绑定用`addEventListener()`方法，
 `return false;`会失效，我们可以`用preventDefault()`方法来代替 return false;

```javascript
function myAddEvent(obj, sEv, fn) {
    //IE浏览器
    if(obj.attachEvent) {
        obj.attachEvent("on" + sEv, function() {
            if(false==fn.call(obj)){
                return false;
            }
        });
        //标准浏览器
    } else 
      {
        obj.addEventListener(sEv, function(ev) {
        //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
         if(false==fn.call(obj))
            {
                ev.preventDefault();
            }
        },false);
    }
}
```
在标准浏览器下测试，没有问题，阻止了右键的弹出事件。

我们不仅要阻止默认事件，我们还要阻止冒泡事件的呀，这里还是得修改myAddEvent()函数，我们需要使用`event.cancelBubble `来阻止冒泡事件。

```javascript
function myAddEvent(obj, sEv, fn) {
    //IE浏览器
    if(obj.attachEvent) {
        obj.attachEvent("on" + sEv, function() {
            if(false == fn.call(obj)) {
                event.cancelBubble = true;
                return false;
            }
        });
        //标准浏览器
    } else {
        obj.addEventListener(sEv, function(ev) {
            //false表示冒泡，true表示捕获，大多数情况下用冒泡不用捕获
            if(false == fn.call(obj)) {
                event.cancelBubble = true;
                ev.preventDefault();
            }
        }, false);
    }
}
//封装on()函数
Yquery.prototype.on = function(sEv, fn) {
    //不同的选择器可能选择多个元素，给每个选中的元素都需要添加on()方法
    var i = 0;
    //循环的给每个元素都绑定事件
    for(i = 0; i < this.elements.length; i++) {
        //给每个元素绑定sEv事件，执行fn函数
        myAddEvent(this.elements[i], sEv, fn);
    }
}
```

我们来测试一个看看是不是能阻止冒泡呢

```javascript
<body>
<div style="background:red;">
    <div style="background:green;">
        <div style="background:#CCC;"></div>
    </div>
</div>
</body>
<script>
$(function (){
    $('div').click(function (){
        console.log(this.style.background);
        return false;
    });
});
</script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_20.png)

至此，我们已经封装好了on()函数，并且可以阻止冒泡和阻止默认事件。

### 20.插件机制

插件机制：其实就是给`Yquery`原型上添加一个方法，

有两个参数，一个是方法的名称，另一个是方法的执行过程。

Yquery.js：

```javascript

Yquery.prototype.extend = function(name, fn)
{
    Yquery.prototype[name] = fn;
};
```

利用`$().extend(function(){})`方法动态添加插件/方法

```javascript
$().extend("name",function(){
})
```
我们可以来测试下这样的方法到底可不可以动态的给Yquery添加方法。

举例：

* 添加size()方法，获取页面中的元素的个数。

```javascript
       <style>
            div {
                width: 50px;
                height: 50px;
                background-color: red;
                float: left;
                margin: 10px;
            }
        </style>
    <body>
        <div><span></span></div>
        <div><span></span></div>
        <div><span></span></div>
        <div><span></span></div>
        <div></div>
        <div></div>
        <div></div>
    </body>
<script type="text/javascript">
            $().extend('size', function() {
                return this.elements.length;
            })
            $(function(){
                console.log($('div').size());
                console.log($('div').find('span').size());
            })
</script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_21.png)
我们可以正确的得到页面中元素的个数，至此我们的Yquery.js就封装好了。

源代码见我的仓库地址：https://github.com/255255255255/Yquery

接下来，我们就可以封装各类的框架，使我们的Yquery.js更加强大。

### 21.animate()插件的封装：`Yquery_animate.js`

`animate()`方法的参数其实只有一个，就是json字符串。

```javascript

$().extend('animate', function (json){
    var i=0;
    //让被选择器选中的元素，也就是elements数组中的每个元素都添加动画效果
    for(i=0;i<this.elements.length;i++)
    {
        startMove(this.elements[i], json);
    }
    function getStyle(obj, attr)
    {
        if(obj.currentStyle)
        {
            return obj.currentStyle[attr];
        }
        else
        {
            return getComputedStyle(obj, false)[attr];
        }
    }
    function startMove(obj, json, fn)
    {
        clearInterval(obj.timer);
        obj.timer=setInterval(function (){
            var bStop=true;     //这一次运动就结束了——所有的值都到达了
            for(var attr in json)
            {
                //1.取当前的值
                var iCur=0;
                if(attr=='opacity')
                {
              iCur=parseInt(parseFloat(getStyle(obj, attr))*100);
                }
                else
                {
                    iCur=parseInt(getStyle(obj, attr));
                }
                //2.算速度
                var iSpeed=(json[attr]-iCur)/8;
                iSpeed=iSpeed>0?Math.ceil(iSpeed):Math.floor(iSpeed);
                //3.检测停止
                if(iCur!=json[attr])
                {
                    bStop=false;
                }
                if(attr=='opacity')
                {
                    obj.style.filter='alpha(opacity:'+(iCur+iSpeed)+')';
                    obj.style.opacity=(iCur+iSpeed)/100;
                }
                else
                {
                    obj.style[attr]=iCur+iSpeed+'px';
                }
            }
            if(bStop)
            {
                clearInterval(obj.timer);
                if(fn)
                {
                    fn();
                }
            }
        }, 30)
    }
});
```
那我们来测试一下吧

需要把`Yquery.js`和`Yquery_animate.js`文件都引入，才会出现效果。

```
<!DOCTYPE html>
<html>
    <head>
        <style>
            #div1 {
                width: 100px;
                height: 100px;
                background: red;
                filter: alpha(opacity:30);
                opacity: 0.3;
            }
        </style>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>无标题文档</title>
        <script src="../Yquery.js"></script>
        <script src="../Yquery_animate.js"></script>
        <script>
            $(function() {
                $('input').toggle(function() {
                    $('#div1').animate({
                        width: 200,
                        height: 200,
                        opacity: 100
                    });
                }, function() {
                    $('#div1').animate({
                        width: 100,
                        height: 100,
                        opacity: 30
                    });
                });
            });
        </script>
    </head>
    <body>
        <input type="button" value="运动" />
        <div id="div1">
        </div>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1236309/o_22.png)

如图所见，出现了我们想要的动画效果，太开心了。和jQuery库中的animate()方法可是一点都不差呢！！可以用animate()方法制作上下焦点图、左右焦点图等，参考我的另一篇博客：在这里就不废话了。
https://yingy0.github.io/2018/06/07/jQuery%E5%AE%9E%E4%BE%8B/

### 22.drag()插件的封装`Yquery_drag.js`

```javascript
$().extend('drag', function (){
    var i=0;
    //让被选择器选中的元素，也就是elements数组中的每个元素都添加拖拽效果
    for(i=0;i<this.elements.length;i++)
    {
        drag(this.elements[i]);
    }
    function drag(oDiv)
    {
        oDiv.onmousedown=function (ev)
        {
            var oEvent=ev||event;
            var disX=oEvent.clientX-oDiv.offsetLeft;
            var disY=oEvent.clientY-oDiv.offsetTop;
            document.onmousemove=function (ev)
            {
                var oEvent=ev||event;
                oDiv.style.left=oEvent.clientX-disX+'px';
                oDiv.style.top=oEvent.clientY-disY+'px';
            };
            document.onmouseup=function ()
            {
                document.onmousemove=null;
                document.onmouseup=null;
            };
        };
    }
});
```

那我们来测试一下：

需要把`Yquery.js`和`Yquery_drag.js`文件都引入，才会出现效果。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            div{
                width: 100px;
                height: 50px;
                background-color: #F47500;
                position: absolute;
            }
        </style>
        <script type="text/javascript" src="../Yquery.js" ></script>
        <script type="text/javascript" src="../Yquery_drag.js" ></script>
        <script>
            $(function(){
                $('div').drag();
            })
        </script>
    </head>
    <body>
        <div></div>
    </body>
</html>
```
注意：一定要给被拖动的元素设置定位(相对定位、固定定位)，才会出现拖动的效果。

源代码见我的仓库地址：https://github.com/255255255255/Yquery

由于在Github中的图片有时不能完全加载，详细请看我的博客https://yingy0.github.io/2018/05/28/Yquery/。