---
title: javaScript客户端与服务器端的五种通信方式
comments: true
date: 2018-07-04 10:06:12
categories: 前端
tags: 客户端与服务器通信方式
about:

---

> 在Web项目中，要实现客户端与服务端的交互，可通过cookie、隐藏框架、使用iframe、HTTP请求、LiveConnect请求和智能HTTP请求等方式实现。下面我就来一一的说下。

一、cookie

 cookie是第一个JavaScript可以利用的客户端-服务端之间的交互手段。浏览器向服务器发送请求时，为这个服务器存储的cookie会与其他信息一起发送到服务器。这使得JavaScript可以在客户端设置一个cookie，之后服务器端就能够读取它了。
 我截取了百度首页的cookie的情况，如下图。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_1.png)

1.cookie的成分

* 名称(name):每一个cookie由一个唯一的名称代表。这个名称可以包含字母、数字和下划线。不区分大小写。

* 值(Value):保存在cookie中的字符串值。这个值在存储之前必须用`encodeURIComponent()`对其进行编码，以免丢失数据或占用了cookie。名称和值加起来的字节数不能超过4095字节，也就是4KB。

`encodeURIComponent()`:函数可把字符串作为 URI 组件进行编码。

语法：`encodeURIComponent(URIstring)`;

参数：`URIstring`:必需。一个字符串，含有 URI 组件或其他要编码的文本。

```javascript
<script type="text/javascript">

document.write(encodeURIComponent("http://www.w3school.com.cn"))
document.write("<br />")
document.write(encodeURIComponent("http://www.w3school.com.cn/p 1/"))
document.write("<br />")
document.write(encodeURIComponent(",/?:@&=+$#"))

</script>

```
输出结果:

```javascript
http%3A%2F%2Fwww.w3school.com.cn
http%3A%2F%2Fwww.w3school.com.cn%2Fp%201%2F
%2C%2F%3F%3A%40%26%3D%2B%24%23
```
* 域(Domain):处于安全考虑，网站不能访问其他域创建的cookie。创建cookie后，域的信息会作为cookie的一部分存储起来。

同源策略是浏览器的一种安全策略，所谓同源是指，域名，协议，端口完全相同。

那什么叫做跨域呢？不同地址，不同端口，不同级别，不同协议都会构成跨域。

举例：

```javascript
URL                      说明       是否允许通信
https://yingy0.github.io/a.js
https://yingy0.github.io/b.js     同一域名下   允许

https://yingy0.github.io/lab/a.js
https://yingy0.github.io/script/b.js 同一域名下不同文件夹 允许

https://yingy0.github.io:8000/a.js
https://yingy0.github.io/b.js     同一域名，不同端口  不允许

http://yingy0.github.io/a.js
https://yingy0.github.io/b.js 同一域名，不同协议 不允许

http://www.baidu.com/a.js
http://119.75.217.109/b.js 域名和域名对应ip

http://www.baidu.com/a.js
http://about.baidu.com/b.js 主域相同，子域不同 不允许

http://www.baidu.com/a.js
http://baidu.com/b.js 同一域名，不同二级域名（同上） 不允许（cookie这种情况下也不允许访问）

http://www.hao123.com/a.js
http://www.haorooms.com/b.js 不同域名 不允许
```
* 路径(path):另一个cookie的安全特性，路径限制了对Web服务器上的特定目录的访问。

* 失效日期(Expires/Max-Age):确定cookie何时删除。默认情况下，关闭浏览器时，
即将cookie删除，不过可以自己设置删除时间。这个值是个GMT格式的日期
（可以使用Date对象的toGMTString()方法），用于制定应该删除cookie的准确时间。
如果设置的日期是当前日期以前的时间，则cookie被立刻删除。
 
* 安全标志(Secure)：用于表示cookie是否只能从安全网站（使用SSL和https协议的网站）中访问。可以将这个值设为true以加强保护，进而确保cookie不被其他网站访问。

2.其他安全限制

* 每个域最多只能只能在一台用户的机器上存储20个cookie；

* 每个cookie的总尺寸不能超过4096字节；
* 一台用户的机器上的cookie总数不能超过30个。

3.举例:

* setCookie方法：给cookie属性上赋值

```javascript

    <script type="text/javascript">
      //setCookie方法：给cookie属性上赋值
      function setCookie(sName, sValue, oExpires, sPath, sDomain, bSecure) {
        var sCookie = sName +"="+encodeURIComponent(sValue);
        if(oExpires) {
          sCookie += ";expires = "+oExpires.toGMTString();
        }
        if(sPath) {
          sCookie += ";path" + sPath;
        }
        if(sDomain) {
          sCookie += ";domain" + sDomain;
        }
        if(bSecure) {
          sCookie += ";secure";
        }
        document.cookie = sCookie;
      }

      //setCookie()函数只有前两个参数是必须的，函数的调用方法如下：

      setCookie('name', "Asci");

      setCookie('book', 'JavaScript', new Date(Date.parse('1 10,2019')));

      setCookie("message", "hello", new Date(Date.parse('1 10,2019')), "/books", 'http://127.0.0.1:8020', true);
    </script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_2.png)

* 另外一种给cookie属性赋值的代码，比较简单大家可以参考着来。

```javascript
<script type="text/javascript">
  function setCookie(key,value,time) {
  var date=new Date();
  date.setDate(date.getDate()+time);
  document.cookie=key+"="+value+";expires"+date.toGMTString();
  }
  setCookie('username','Asci',7);
  setCookie('password','123456',7)
    
</script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_3.png)

* 下一次打开浏览器的时候，利用getCookie()方法.可以请求上次的cookie数据

```javascript
<script type="text/javascript">
  function getCookie(sName) {
    var arr = document.cookie.split(";");
    for(var i = 0; i < arr.length; i++) {
      var kv = arr[i].split('=');
      if(kv[0] == sName) {}
      return kv[1];
    }
  }
  //调用该方法可以获取指定名称的cookie，调用举例如下：
  var sName = getCookie("name");
  var sName = getCookie("book");
</script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_4.png)

当获取到用户名和密码之后，调用ajax请求，直接可以向服务端发送请求，
输入用户名和密码的过程通过自动处理从本地文件已经获取了，不需要手动输入了，表现的形式就是自动登录。

* 删除cookie的数据用deleteCookie()方法，只需将失效时间设为过去的一个时间即可。

```javascript

function deleteCookie(sName, sPath, sDomain) {

 setCookie(sName, "", new Date(0), sPath, sDomain);

}

```
4.JavaScript中的cookie

doucment对象有个cookie特性，是包含所有给定页面可访问的cookie的字符串，cookie特性设置为新值只会改变对页面可访问的cookie，并不会真正改变cookie本身。

cookie字符串的格式：

```javascript
cookie_name=cookie_value
expires=expiration_time
path=domain_path
domain=d
secure
setCookie()函数
getCookie()函数
deleteCookie()函数
```
5.服务器端的cookie：JSP

 Jsp提供了非常简单的处理cookie的接口，request对象会在执行JSP时自动初始化，有一个返回一个Cookie对象数组的方`getCookies()`方法。每个Cookie对象都具有`getName()`，`getPath()`，`getDomain()`，`getSecure()`，`getMaxAge()`等方法。

举例：

* getCookie()方法：请求cookie的值

```javascript
public static Cookie getCookie(HttpServletRequest request, String name) {

       Cookie[] cookies = request.getCookies();

       if (cookies != null) {
       for (int i = 0; i<cookies.length; i++) {

       if (cookies[i].getName().equals(name)) {

       return cookies[i];
          }

      }

    }
}
```
* 新建一个cookie：

```javascript
Cookie nameCookie = new Cookie("name", "Asci");

nameCookie.setDomain("https://yingy0.github.io/");

nameCookie.setPath("/books");

response.addCookie(nameCookie);
```
* 删除cookie：

```
Cookie cookieToDelete = getCookie("name");

cookieToDelete.setMaxAge(0);  //设置有效时间为0

response.addCookie(cookieToDelete);
```
二、隐藏框架

创建一个可用JavaScript与服务器进行通信的0像素高的框架,用于处理客户端通信的JavaScript对象和在服务端处理通信的特殊页面。

方法：

```JavaScript
getServerInfo()：发送请求函数

handleResponse()：在隐藏框架从服务器返回页面之后调用
```
发送请求的函数如下：

```javascript
//向服务器发送请求
function getServerInfo() {

    parent.frames["hiddenFrame"].location.href = "hiddenFrameCom.html";

}
```
处理回应的函数`handleResponse()`如下：

```javascript
function handleResponse(sResponseTextt) {

    alert("服务器返回:" + sResponseTextt);

}
```
处理隐藏请求的页面必须输出一个普通的HTML页面，需要使用`<textarea/>`元素，方便的处理多行数据，这个页面必须在主框架中调用`handleResponse()`函数。

```javascript
<html>

  <head>
<title>
  隐藏框架的例子</title>
<script type="text/javascript">
  window.onload = function() {
    parent.frames[0].handleResponse(
      document.forms["formResponse"].result.value);
  };
</script>
</head>
  <body>
    <form name="formResponse">
      <textarea name="result">传送回的数据</textarea>
    </form>
  </body>
</html>
```
三、HTTP请求

 现在很多浏览器都可以直接从JavaScript中初始化HTTP请求并获取结果，完全不用隐藏框架和其他取巧的小技巧。核心是，微软创建的XMLHTTP请求的对象。这个对象是与MSXML一起出现的，XMLHTTP请求实质上是添加了额外的用于发送和接收XML代码的功能的普通的HTTP请求。

* 在IE中重新创建`XMLHTTP`请求对象，需要使用`ActiveXObject`，如下所示：

`var oRequest = new ActiveXObject("");`

* 标准浏览器创建XMLHTTP请求对象

`var oRequest = new XMLHttpRequest("");`

* 在IE浏览器中创建`XMLHTTP()`的方法：

```javascript
<script type="text/javascript">
function createXMLHTTP() {
    var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
"MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP","Microsoft.XMLHTTP"];
       for (var i=0; i< arrSignatures.length; i++) 
       {
              var oRequest = new ActiveXObject(arrSignatures[i]);
              return oRequest;
       }
     }
</script>
```
创建好HTTP请求后可用`open()`方法来指定要发送的请求,参数如下：

* 第一个参数：值可为"get"或"post"，或其他受服务器支持的HTTP方法；

* 第二个参数：请求的URL地址;

* 第三个参数：表示请求是否以异步方式发送的布尔值。true表示以异步的方式，false表示以同步的方式。

举例：`oRequest.open("get", "example.txt", false);`

打开后，可用send()方法将请求发送出去，该方法必须带一个参数，该参数一般情况下值为null。

举例:`oRequest.send(null);`

一个完整的栗子：创建、打开、发送(同步发送)。

```javascript
    <script type="text/javascript">
      function createXMLHTTP() {
        var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
          "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP"
        ];
        for(var i = 0; i < arrSignatures.length; i++) {
          //我在Chrome浏览器下运行，所以用的是XMLHttpRequest,IE浏览器的朋友们用的是ActiveXObject
          var oRequest = new XMLHttpRequest(arrSignatures[i]);
          return oRequest;
        }
      }
      var oRequest = createXMLHTTP();

      oRequest.open("get", "example.txt", false); //以同步的方式发送数据

      oRequest.send(null);

      console.log("状态:" + oRequest.status + "(" + oRequest.statusText + ")");

      console.log("回应的文本信息:" + oRequest.responseText);
    </
script>
```
结果会获取服务器端的一个纯文本文件`(example.txt)`，也就是说以同步的方式请求该txt文件，然后在浏览器中显示此内容。如果使用同步调用（第三个参数为false），则JavaScript解释程序就会等待请求返回,返回响应后，status特性中会放入请求的HTTP状态，同时在`statuesText`特性中放入描述状态的信息，在`responseText`特性放入服务器上接收到的文本。

运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_5.png)

如果发送异步请求，必须使用onreadystatechange事件处理函数，并检查readyState特性是否等于4。

举例：

```javascript
    <script type="text/javascript">
      function createXMLHTTP() {
        var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
          "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP"
        ];
        for(var i = 0; i < arrSignatures.length; i++) {
          var oRequest = new XMLHttpRequest(arrSignatures[i]);
          return oRequest;
        }
      }
      var oRequest = createXMLHTTP();

      oRequest.open("get", "example.txt", true); //以异步的方式发送数据

      oRequest.onreadystatechange = function() {

          if(oRequest.readyState == 4) {

          console.log("状态:" + oRequest.status + "(" + oRequest.statusText + ")");

          console.log("回应的文本信息:" + oRequest.responseText);

          }
   }
          oRequest.send(null);
    </script>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232440/o_5.png)

1.使用HTTP头部

每个HTTP请求发送时都包含一组带有额外信息的首部，在XML HTTP请求对象提供了获取和设置它们的方法

* `getAllResponseHeaders()`：返回包含所有响应的HTTP首部信息的字符串；

* `getResponseHeader()`：获取指定的某个头部，参数为获取的首部的名称，

`var sValue = oRequest.getResponseHeader("Server");`

* setResponseHeader()：设置XML HTTP请求的首部信息，

`oRequest. setResponseHeader("myheader", "Asci");`

2.实现的复制品

Mozilla第一个复制了XMLHTTP实现，创建了名为XMLHTTPRequest的JavaScript，行为完全与微软的版本相同，Opera(7.6)和Safari（1.2）也复制了Mozilla的实现，创建了自己的XMLHTTPRequest对象。

3.进行GET请求

Web上最常见的请求类型就是GET请求，每次在浏览器中输入URL并打开页面时，就是像服务器发送一个GET请求,GET请求的参数就是用问号追加到URL的结尾，后面跟着用&号连接起来的"名称/值"。

每个名称和值都必须在编码后才能用在URL中,在JavaScript中可以用`encodeURI Component()`进行编码.URL最大长度为2048字符,问号后面的内容为查询字符串，这些参数可以在服务器端的页面中读取。

要是用XMLHTTP请求对象发送一个GET请求，只需将URL(包含所有的参数)传入open()方法，同时第一个参数设为“get”,参数必须追加到URL的末尾.

`addURLParam()`函数有三个参数:

* 要添加参数的URL

* 参数名称

* 参数值

首先，函数检查URL中是否已经存在一个问号（存在就表示已经有参数了）。如果没有，函数就追加一个问号，否则就追加一个&号。

举例：

```
    <script type="text/javascript">
      function createXMLHTTP() {
        var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
          "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP"
        ];
        for(var i = 0; i < arrSignatures.length; i++) {
          var oRequest = new XMLHttpRequest(arrSignatures[i]);
          return oRequest;
        }
      }
      var oRequest = createXMLHTTP();
      //为了添加参数的方便性，让我们增加一个添加参数的方法，然后为请求构建一个URL地址
      function addURLParam(url, sParamName, sParamValue) {
        url += (url.indexOf(" ? ") == -1 ? " ? " : " & ");
        url += encodeURIComponent(sParamName) + " = " + encodeURIComponent(sParamValue);
        return url;
      }
      var url = "https://yingy0.github.io/";
      url = addURLParam(url, "gender", "女");
      url = addURLParam(url, "age", "25");
      oRequest.open("get", url, false);
    </script>
```
一般情况下，用同步的方式请求不安全，最好用异步的方式请求。

```javascript
<script type="text/javascript">
  function createXMLHTTP() {
    var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
      "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP"
    ];
    for(var i = 0; i < arrSignatures.length; i++) {
      var oRequest = new XMLHttpRequest(arrSignatures[i]);
      return oRequest;
    }
  }
  var oRequest = createXMLHTTP();

  oRequest.open("get", "example.txt", true); //以异步的方式发送数据

  oRequest.onreadystatechange = function() {

    if(oRequest.readyState == 4) {
 //为了添加参数的方便性，让我们增加一个添加参数的方法，然后为请求构建一个URL地址
      function addURLParam(url, sParamName, sParamValue) {
        url += (url.indexOf(" ? ") == -1 ? " ? " : " & ");
        url += encodeURIComponent(sParamName) + " = " + encodeURIComponent(sParamValue);
        return url;
      }
    }
    var url = "https://yingy0.github.io/";
    url = addURLParam(url, "gender", "女");
    url = addURLParam(url, "age", "25");
    oRequest.open("get", url, false);
  }
</script>
```
4.进行`POST请求`

`POST请求`用于在表单中输入数据后的提交过程，因为`POST`可以比`GET方式`发送更多数据（最多2GB），POST请求的参数也必须进行编码，并用&进行分割，但是这些参数不会被附加到URL上。

```javascritp
<script type="text/javascript">
  function createXMLHTTP() {
    var arrSignatures = ["MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0",
      "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP"
    ];
    for(var i = 0; i < arrSignatures.length; i++) {
      var oRequest = new XMLHttpRequest(arrSignatures[i]);
      return oRequest;
    }
  }
  var oRequest = createXMLHTTP();
  oRequest.open("get", "example.txt", true); //以异步的方式发送数据
  oRequest.onreadystatechange = function() {
    if(oRequest.readyState == 4) {
      function addPostParam(sParams, sParamName, sParamValue) {
        if(sParams.length > 0) {
          sParams += " &";
        }
        return sParams + encodeURIComponent(sParamName) + "= " + encodeURIComponent(sParamValue);
      }
      var sParams = "";
      sParams = addPostParam(sParams, "gender", "女");
      sParams = addPostParam(sParams, "age", "25");
      oRequest.open("open", "example.txt", false);
      oRequest.send(sParams);

    }

   }
</script>
```

注意：该小节所有关于HTTP请求的代码都在本地根目录下面运行，没有配置服务器环境的同学们，请配置好环境之后，再来测试该代码。

四、`LiveConnect`请求

 `LiveConnect`由`Netscape Navigator`引入，一般可以让JavaScript与Java类实现交互的能力。用户必须安装JRE,并且还需在浏览器中启用Java。

1.进行GET请求：

* 创建`java.net.URL`的实例

* 使用`Live Connect`时，必须提供类的完整名称，才能初始化一个Java对象。

* 创建URL后，就可以打开一个输入流并使用读取器来读取数据。缓冲阅读器是逐行获取数据的，要创建一个变量来将响应组成完整的文本,响应文本的变量必须是空字符串`" "`.

* 最好的方法是创建一个`InputStreamReader`，然后再基于它创建一个`BufferReader`。

```
function httpGet(url) {

    var ourl = new java.net.URL(url);

    var oStream = ourl.openStream();

    var oReader = new java.io.BufferedReader(new java.io.InputStreamReader(oStream));

    var oResponseText =" ";

    var sLine = oReader.readLine();

    while (sLine != null) {

       oResponseText += sLine + "\n";
       sLine = oReader.readLine();
}
    oReader.close();
    return oResponseText;
}
```
注意：与`XMLHTTP`请求对象不同，`LiveConnect`要求输入完整的请求的URL，从`http://`开始，这是因为，这个Java对象没有任何解释相对URL的上下文。

2.进行POST请求

* 使用`Connection对象`来协助进行请求确定连接对象的设置.

* 因为POST请求可看作是双向的，所以必须使用`setDoInput()`和`setDoOutput()`方法将连接设成接受输入和输出。

* 另外，连接不应该使用任何缓存数据，所以要调用setUseCaches(false)。

* 必须用`setRequestProperty()`方法将`"Content-Type"`设为相应的值。

* 建立连接后获取请求的输出流,利用`writeBytes()`方法：输出流。

* 从连接中取得输入流并逐行读取数据。

```
function httpPost(url, sParams) {
    var ourl = new java.net.URL(url); //创建java.net.URL的实例

    var oConnection = ourl.openConnection();

    oConnection.setDoInput(true);

    oConnection.setDoOutput(true);

    oConnection.setUseCaches(false);

    oConnection.setRequestProperty("Content-Type","application/x-www-form-urlencoded");

    var output = new java.io.DataOutputStream(oConnection.getOutputStream());

    output.writeBytes(sParams);

    output.flush();

    output.close();

    var sLine ="", sResponseText = "";
    var input = new java.io.DataInputStream(oConnection.getInputStream());
       sLine = input.readLine();
           while (sLine != null) {

              sResponseText += sLine + “\n”;

              sLine = input.readLine();
         } 
       input.close();
       return oResponseText;
}
```
五、智能HTTP请求

检测是否要使用`XMLHTTP`请求对象，判断`XMLHTTPRequest`的类型是否等于`"object"`(针对标准浏览器)，以及`window.ActiveXObject`(针对IE浏览器)是否有效

`get()`方法：用于对指定的URL进行一个GET请求，有两个参数：

* 发送请求的URL

* 一个回调函数

* 传给这个回调函数的唯一参数是从HTTP请求中获取的数据(sData)。

智能HTTP请求的步骤：

* 首先用`XMLHTTP`请求的对象进行GET请求时，使用异步请求可以设置另一个回调函数，同时在`readyState`为4时，调用上述的回调函数。

* `onreadystatechange`事件处理函数调用指定的回调函数`fnCallback`。

* 如果浏览器不支持`XMLHTTP`请求，就必须检查是否用了`LiveConnect`，没有任何特性或者设置能够表示`LiveConnect`是否可用，唯一的办法是用`navigator.javaEnabled()`方法来保证已经浏览器中的java,并）判断java和`java.net`是否已定义。

* 如果要模仿异步调用，可用`setTimeout()`来延迟这一段时间，然后在调用回到函数。

* 因为LiveConnect要求必须输入完整的URL，所以每次都必须提供完整的URL给`Http.get()`方法

```
var bXmlHttpSupport = (typeof XMLHttpRequest == “object” || window.ActiveXObject);

Http.get = function (url, fnCallback) {

       if (bXmlHttpSupport) {

       var oRequest = new XMLHttpRequest();

       oRequest.open(“get”, url, true);

       oRequest.onreadystatechange = function() {

       if (oRequest.readyState == 4) {

       fnCallback(oRequest.responseText);
  }
}
oRequest.send(null);
} 
else if(navigator.javaEnabled()&&typeof java != "undefined"

&& type java.net != "undefined") {

setTimeout(function() {

       fnCallback(httpGet(url));

}, 10);

} else {
       alert("你的浏览器不支持HTTP请求!");
  }
}
```
`post()`方法：对请求首部的设置、参数的数量以及方法发送的是一个`POST`请求，除了需要三个参数（URL、参数字符串和回调函数）外，`post()`方法类似于get()方法，大家可以参照get()方法来对比着看，不过get()只需要两个参数。

```javascript
var bXmlHttpSupport = (typeof XMLHttpRequest == “object” || window.ActiveXObject);
Http.post = function(url, sParams, fnCallback) {

        if (bXmlHttpSupport) {

       var oRequest = new XMLHttpRequest();

       oRequest.open(“post”, url, true);

       oRequest.setRequestHeader(“Content-Type”,“application/x-www-form-urlencoded”);

       oRequest.onreadysatechange = function() {

       if (oRequest.readyState == 4) {

              fnCallback(oRequest.responseText);
     }
  }
} else if ((navigator.javaEnabled() && typeof java != “undefined”

&& type java.net != “undefined”) {

       setTimeout(function() {

       fnCallback(httpPost(url, sParams));

}, 10);

} else {
       alert(“你的浏览器不支持HTTP请求!”);
  }
}
```


