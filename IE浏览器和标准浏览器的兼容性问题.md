---
title: IE浏览器和标准浏览器的兼容性问题
comments: true
date: 2018-06-06 14:43:31
categories: 博客列表
tags: 浏览器兼容性
about:

---

### 1、`document.formName.item("itemName") `

IE浏览器：可以使用`document.formName.item("itemName")`或`document.formName.elements["elementName"]`;

标准浏览器：只能使用`document.formName.elements["elementName"]`。

解决方法：统一使用`document.formName.elements["elementName"]`。

2、HTML对象获取问题

标准浏览器：可以使用`document.getElementById("idName")`

IE浏览器使用`document.idname`或者`document.getElementById("idName")`

解决办法：统一使用`document.getElementById("idName")`;

3、事件委托方法

IE浏览器：使用`document.body.onload = inject; `其中`function inject()`在这之前已被实现,在这里只是调用一下;

标准浏览器：使用`document.body.onload` = inject();

解决方法：统一使用`document.body.onload=new Function('inject()')`; 或者`document.body.onload = function(){}`

4、集合类对象

IE浏览器：可以使用`()`或`[]`获取集合类对象；

标准浏览器：只能使用`[ ]`获取集合类对象。

解决方法：统一使用`[]`获取集合类对象。

5、自定义属性问题

IE浏览器：可以使用获取常规属性的方法来获取自定义属性,也可以使用`getAttribute()`获取自定义属性

标准浏览器：只能使用`getAttribute()`获取自定义属性。

解决方法：统一通过`getAttribute()`获取自定义属性，使用"."点运算符访问更加方便。

6、`eval("idName")`问题

IE浏览器：可以使用`eval("idName")`或`getElementById("idName")`来取得id为idName的HTML对象

标准浏览器：只能使用`getElementById("idName")`来取得id为idName的HTML对象。

解决方法：统一用`getElementById("idName")`来取得id为idName的HTML对象。

7、变量名与某HTML对象ID相同的问题

IE浏览器：HTML对象的ID可以作为document的下属对象变量名直接使用;标准浏览器只能通过`getElementById`获取HTML5对象的ID值.

标准浏览器：可以使用与HTML对象ID相同的变量名;IE浏览器下则不能。

解决方法：使用`document.getElementById("idName")`代替`document.idName`，并且最好不要取HTML对象ID相同的变量名，以减少错误;在声明变量时，一律加上var,以避免歧义。

8、const问题

标准浏览器：可以使用`const`关键字或var关键字来定义常量。

IE浏览器：只能使用`va`r关键字来定义常量。

解决方法：统一使用`var`关键字来定义常量。

9、`input.type`属性问题

IE浏览器：`input.type`属性为只读

标准浏览器：`input.type`属性为可读可写。

解决方法：可以分别创建输入框，让输入框尽可能只展现可读属性，尽量隐藏它的可写属性，因为IE浏览器浏览器不支持，想通过把type为password的值改为text，IE浏览器下是不允许的。

10、`window.event`问题

`window.event`只能在IE浏览器下运行，而不能在标准浏览器下运行,这是因为标准浏览器的event只能在事件发生的现场使用。标准浏览器必须从源处加入event作参数传递。IE浏览器忽略该参数，用`window.event`来读取该event。

解决方法： `var e = e || window.event`

举例：在IE浏览器下获得鼠标位置的方法

```
<script>
<button>获得鼠标点击横坐标</button>
<script type="text/javascript">
   var btn=document.getElementsByTagName("button");
      btn.onclick=function(){
          alert(event.clientX);
      }
</script>
```

兼容式的获得鼠标位置的方法

```javascript
<script>
<button>获得鼠标点击横坐标</button>
<script type="text/javascript">
   var btn=document.getElementsByTagName("button");
     btn.onclick=function(event){
       event = event || window.event;
        alert(event.clientX);
    }
</script>
```

11、event.x与event.y问题

IE浏览器：even对象有x,y属性,但是没有pageX，pageY属性

标准浏览器：even对象有pageX，pageY属性，但是没有x,y属性。

解决方法：使用`var x = e.x ? e.x : e.pageX; `来代替IE浏览器下的event.x或者标准浏览器下的e.pageX；

12、event.srcElement问题

IE浏览器：event对象有srcElement属性,但是没有target属性;

标准浏览器：even对象有target属性,但是没有srcElement属性。

解决方法：使用`obj(obj = event.srcElement ? event.srcElement : event.target;)`来代替IE浏览器下的`event.srcElement`或者标准浏览器下的`event.target`，请同时注意event的兼容性问题(第10个)。

13、window.location.href问题

IE浏览器或者标准浏览器较高版本：可以使用window.location或window.location.href；

标准浏览器旧版本：只能使用`window.location`。

解决方法：使用`window.location`来代替`window.location.href`。

14、模态和非模态窗口问题

IE浏览器：可以通过showModalDialog和showModelessDialog打开模态和非模态窗口，标准浏览器下则不能。

解决方法：直接使用`window.open(pageURL,name,parameters)`方式打开新窗口。

如果需要将子窗口中的参数传递回父窗口，可以在子窗口中使用`window.opener`来访问父窗口。如果需要父窗口控制子窗口的话，使用
`var subWindow = window.open(pageURL,name,parameters)`; 来获得新开的窗口对象。

如果需要将frame中的参数传回父窗口，可以在frame中使用parent关键字来访问父窗口。

15、body载入问题

标准浏览器：body在body标签没有被浏览器完全读入之前就存在

IE浏览器：body必须在body标签被浏览器完全读入之后才存在


16、table操作问题

IE浏览器和标准浏览器对于`<table>标签`的操作都各不相同，

IE浏览器中不允许对`<table>`标签和`<tr>`的`innerHTML`赋值，并且如果js增加一个tr时不支持使用`appendChild`方法。

标准浏览器：支持：`document.appendChild`方法

解决办法：把行插入到TBODY中，不要直接插入到表

```javascrpt
//向table追加一个空行：
var row = otable.insertRow(-1);
var cell = document.createElement("td");
cell.innerHTML = "";
cell.className = "XXXX";
row.appendChild(cell);
```

17、获取table的行数和列数

在IE中，获取行列数可以使用下面的代码：

```javascript
var detailT= grid.gettable("11");
  //获取行的长度
  var r=detailT.rows.length;
  //获取列的长度
  var l=detailT.cells.length;
```

毫无疑问，在标准的浏览器，这样的方式获取列的长度就是无效的。

解决方案：

```javascript
var detailT= grid.gettable("11");

//获取行的长度
var r=detailT.rows.length;
//获取列的长度
var l=detailT.rows[0].cells.length;
```

18、访问父元素的区别

IE浏览器：支持`parentElement`和`parentNode`获取父节点，

标准浏览器：只支持`parentNode`。

解决方法：因为firefox与IE都支持DOM，因此全部使用`obj.parentNode`;


19、`children`与`childNodes`

标准浏览器：`childNodes`会把换行和空白字符都算作父节点的子节点。

IE浏览器：`childNodes`和`children不会把换行和空白字符都算作父节点的子节点，会直接忽略。

举例：

```javascript
<div id="dd">
    <div>yizhu2000</div>
</div>
```

ID值为dd的div在IE浏览器用childNodes查看，其子节点数为1，而在标准浏览器其子节点数为3，我们可以从标准浏览器的dom查看器里面看到他的childNodes为`["\n ", div, "\n"]`。

解决办法：

```javascript
if (typeof(HTMLElement) != "undefined" && !window.opera) {
    HTMLElement.prototype.__defineGetter__("children", function() {
       for (var a = [], j = 0, n, i = 0; i < this.childNodes.length; i++) {
           n = this.childNodes[i];
           if (n.nodeType == 1) {
                a[j++] = n;
             if (n.name) {
               if (!a[n.name])
                  a[n.name] = [];
                  a[n.name][a[n.name].length] = n;
                }
                 if (n.id)
                   a[n.id] = n;
              }
           }
       return a;
      });
  }
```
20、对象宽高赋值问题

标准浏览器中类似 `obj.style.height = imgObj.height` 的语句无效。

解决方法：统一使用 `obj.style.height = imgObj.height + 'px'`;

21、`setAttribute('style','color:red;')`

标准浏览器：支持`setAttribute('style','color:red;')`

IE浏览器：不支持`setAttribute('style','color:red;')`

解决方法：使用`object.style.cssText = ‘color:red;'`

22、类名设置`setAttribute('class','styleClass')`

标准浏览器：支持`setAttribute('style','color:red;')`，指定属性名为class。

IE浏览器：不支持`setAttribute('style','color:red;')`

，IE浏览器不会设置元素的class属性，相反只使用`setAttribute时IE`获取className属性。

解决方法：

```javascript
setAttribute('class','styleClass') 

setAttribute('className','styleClass') 

或者直接 object.className='styleClass';
```

IE浏览器和标准浏览器都支持`object.className`。

23、用`setAttribute`设置事件

标准浏览器：支持用`setAttribute`设置事件

举例：

```javascript
var obj = document.getElementById('objId');
   obj.setAttribute('onclick','funcitonname();');
```

IE浏览器：不支持用`setAttribute`设置事件

解决办法：IE浏览器中必须用点记法来引用所需的事件处理程序,并且要用赋予匿名函数

举例：

```javascript
var obj = document.getElementById('objId'); 
    obj.onclick=function(){fucntionname();};
```
这种形式所有浏览器都支持，不存在兼容性问题。

24、`innerText`问题

IE浏览器：支持`innerText`

标准浏览器：不支持`innerText`，但是支持`textContent`。

解决方法：`elem.innerText = elem.textContent = "值"`

举例：

```javascript
if (navigator.appName.indexOf("Explorer") > -1) {
document.getElementById('element').innerText = "my text";
} else {
document.getElementById('element').textContent = "my text";
}
```

25、样式关键字冲突问题

CSS属性与Javascript中的保留关键字命名相同，IE浏览器中style+属性，标准浏览器中css+属性。

26、`removeChild`和`removeNode`的问题

IE浏览器：支持`appendNode`和`removeNode`。

标准浏览器：支持`removeChild`。

27、事件监听函数的问题

标准浏览器的写法：`addEventListener()`

IE浏览器的写法：`attachEvent()`

解决方法：判断`addEventListener`是否存在，如果存在则用否则用IE浏览器8以下的版本的绑定方法`attachEvent`，`removeEventListener()`和`detachEvent()`也是一样的用法。

28、`AJAX`获取`XMLHTTP`的区别

IE浏览器：用`window.ActiveXObject`获取`XMLHTTP`

标准浏览器：用`window.XMLHttpRequest`获取`XMLHTTP`

解决办法：

```javascript
 var xhr;
   if (window.XMLHttpRequest) {
        xhr = new XMLHttpRequest();
    } elseif (window.ActiveXObject) { // IE的获取方式
         xhr = new ActiveXObject("Microsoft.XMLHTTP");
   }
```

29、阻止事件冒泡

标准浏览器：`stopPropagation()`

IE浏览器：`cancelBubble`

解决方法：判断`stopPropagation`是否存在，如果存在则用标准写法否则则用IE浏览器的写法。

30、阻止默认事件

标准浏览器：`preventDefault()`

IE浏览器：`returnValue()`

解决方法：一般情况可以直接使用`return false`阻止，虽然可以达到相同的效果，但和取消默认事件的意义并不是相同的

31、css()方法可以获取指定元素集合中第一个元素的样式属性的计算值。

标准浏览器：通过的 `getComputedStyle() `方法取得某些属性

IE浏览器：通过`currentStyle`和`runtimeStyle`属性取得某些特定的属性

32、float属性

IE浏览器的DOM 将float属性写成`styleFloat`实现，

标准浏览器将float 属性写成`cssFloat`。

解决方法：使用`"float"`，那就需要引入jQuery库，jQuery将为每个浏览器返回它需要的正确值。

33、frame问题

`<frame src="xxx.html" id="frameId" name="frameName" />`

* 访问`frame`对象

IE浏览器：使用`window.frameId`或者`window.frameName`来访问这个frame对象，`frameId`和`frameName`可以同名;

标准浏览器：只能使用`window.frameName`来访问这个frame对象；

在IE浏览器和标准浏览器中都可以使用`window.document.getElementById("frameId")`来访问这个frame对象；

* 切换`frame`内容

在IE浏览器和标准浏览器中都可以使用下面的方式来切换frame的内容；

```javascript
window.document.getElementById("testFrame").src = "xxx.html"

或

window.frameName.location = "xxx.html"
```

34、建立单选钮

标准浏览器：

```
var rdo = document.createElement('input'); 
rdo.setAttribute('type','radio'); 
rdo.setAttribute('name','radiobtn'); 
rdo.setAttribute('value','checked');
```

IE浏览器：

```
var rdo =document.createElement(”<input name=”radiobtn” type=”radio” value=”checked” />”);
```

这一点区别和前面的都不一样。这次完全不同，所以找不到共同的办法来解决，那么只有`IF-ELSE`了
万幸的是，IE可以识别出`document的uniqueID`属性，别的浏览器都不可以识别出这一属性 