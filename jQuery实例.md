---
title: jQuery实例
comments: true
date: 2018-06-29 15:48:54
categories: 前端
tags: JavaScript框架
about:

---

## 1.动态创建表格

* 功能：当点击按钮的时候才会创建表格。

* 重点：Jquery对象转换成DOM对象。然后使用DOM对象的`innerHTML()`方法

```
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
            }
            table {
                border-collapse: collapse;
                border-spacing: 0;
                border: 1px solid #c0c0c0;
            }
            th,
            td {
                border: 1px solid #d0d0d0;
                color: #404060;
                padding: 10px;
            }
            th {
                background-color: #09c;
                font: bold 16px "微软雅黑";
                color: #fff;
            }
            td {
                font: 14px "微软雅黑";
            }
            tbody tr {
                background-color: #f0f0f0;
            }
            tbody tr:hover {
                cursor: pointer;
                background-color: #fafafa;
            }
        </style>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script>
            // 模拟从后台拿到的数据
            var data = [{
                name: "天使在人间博客",
                url: "https://www.yingy0.github.io",
                type: "个人博客"
            }, {
                name: "百度",
                url: "http://www.baidu.com",
                type: "中国最大的搜索引擎"
            }, {
                name: "Github",
                url: "http://github.com",
                type: "开源"
            }];
            $(function() {
                $("#j_btnGetData").click(function() {
                    var tbHtml = "",
                        dataLen = data.length,
                        i = 0;
                    for(; i < dataLen; i++) {
                        tbHtml += "<tr>";
                        tbHtml += "<td>" + data[i].name + "</td>";
                        tbHtml += "<td>" + data[i].url + "</td>";
                        tbHtml += "<td>" + data[i].type + "</td>";
                        tbHtml += "</tr>";
                    }
                    $("#j_tbData")[0].innerHTML = tbHtml;
                });
            });
        </script>
    </head>
    <body>
        <input type="button" value="获取数据" id="j_btnGetData" />
        <table>
            <thead>
                <tr>
                    <th>标题</th>
                    <th>地址</th>
                    <th>说明</th>
                </tr>
            </thead>
            <tbody id="j_tbData">
            </tbody>
        </table>
    </body>
</html>
```
运行效果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_1.gif)

* 注意：jQuery库本身有`$("#j_tbData").html`方法可以将内容动态添加到浏览器中，把jQuery对象转化为DOM对象的主要用途就是为了使用DOM对象中的某些方法。

## 2.手风琴效果

* 功能：当点击标题的时候，会让标题后面的一个div显示出来，并且让其他所有兄弟的元素隐藏起来。

* 重点：`parent()、siblings()、children()、hide()、next()、show()`

```javascript
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
            }
            ul {
                list-style-type: none;
            }
            .parentWrap {
                width: 200px;
                text-align: center;
            }
            .menuGroup {
                border: 1px solid #999;
                background-color: #e0ecff;
            }
            .groupTitle {
                display: block;
                height: 20px;
                line-height: 20px;
                font-size: 16px;
                border-bottom: 1px solid #ccc;
                cursor: pointer;
            }
            .content {
                height: 200px;
                background-color: #fff;
                display: none;
            }
        </style>

        <script src="../js/jquery.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {

                $(".groupTitle").click(function() {
                    //$(".content").hide();
                    $(this).parent(".menuGroup").siblings("li").children("div").hide();
                    //让当前元素后面的div展示出来
                    $(this).next("div").show();

                });

            });
        </script>
    </head>

    <body>
        <ul class="parentWrap">
            <li class="menuGroup">
                <span class="groupTitle">标题1</span>
                <div class="content">我是弹出来的div1</div>
            </li>
            <li class="menuGroup">
                <span class="groupTitle">标题2</span>
                <div class="content">我是弹出来的div2</div>
            </li>
            <li class="menuGroup">
                <span class="groupTitle">标题3</span>
                <div class="content">我是弹出来的div3</div>
            </li>
            <li class="menuGroup">
                <span class="groupTitle">标题4</span>
                <div class="content">我是弹出来的div4</div>
            </li>
        </ul>
    </body>

</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_2.gif)

## 3.tab栏操作

* 功能：鼠标经过某个元素，当前元素样式和内容可以展现出来，其他的兄弟节点的样式和内容隐藏。

* 重点：mouseenter()、addClass()、siblings()、removeClass()、index()、eq()、show()、hide()

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }
        ul {
            list-style: none;
        }
        .wrapper {
            width: 1000px;
            height: 475px;
            margin: 0 auto;
            margin-top: 100px;
        }

        .tab {
            border: 1px solid #ddd;
            border-bottom: 0;
            height: 36px;
            width: 320px;
        }

        .tab li {
            position: relative;
            float: left;
            width: 80px;
            height: 34px;
            line-height: 34px;
            text-align: center;
            cursor: pointer;
            border-top: 4px solid #fff;
        }

        .tab span {
            position: absolute;
            right: 0;
            top: 10px;
            background: #ddd;
            width: 1px;
            height: 14px;
            overflow: hidden;
        }

        .products {
            width: 1002px;
            border: 1px solid #ddd;
            height: 476px;
        }

        .products .main {
            float: left;
            display: none;
        }

        .products .main.selected {
            display: block;
        }

        .tab li.active {
            border-color: red;
            border-bottom: 0;
        }

    </style>
    <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
    <script type="text/javascript">
        $(document).ready(function(){
            // 给tab栏菜单绑定鼠标经过事件
           $(".tab>li").mouseenter(function(){
                // 让当前元素添加样式，让兄弟元素删除样式
                $(this).addClass("active").siblings().removeClass("active");
               // $(this):表示的是让DOM对象转化为Jquery对象,
               //DOM对象转化为Jquery对象的方法是用括号"()"包起来，然后加一个"$"符号。
               // 让当前tab栏对应的内容展示出来，让内容的兄弟隐藏掉
                // 获取当前元素的索引号
                 var index=$(this).index();
                 $(".main").eq(index).show().siblings().hide();
          });
        });
    </script>
</head>
<body>
<div class="wrapper">
    <ul class="tab">
        <li class="tab-item active">国际大牌<span>◆</span></li>
        <li class="tab-item">国妆名牌<span>◆</span></li>
        <li class="tab-item">清洁用品<span>◆</span></li>
        <li class="tab-item">男士精品</li>
    </ul>
    <div class="products">
        <div class="main selected">
            <a href="###"><img src="imgs/guojidapai.jpg" alt=""/></a>
        </div>
        <div class="main">
            <a href="###"><img src="imgs/guozhuangmingpin.jpg" alt=""/></a>
        </div>
        <div class="main">
            <a href="###"><img src="imgs/qingjieyongpin.jpg" alt=""/></a>
        </div>
        <div class="main">
            <a href="###"><img src="imgs/nanshijingpin.jpg" alt=""/></a>
        </div>
    </div>
</div>
</body>
</html>
```
运行效果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_3.gif)


## 4.淘宝精品服装广告

从图中可以看出左侧的菜单项有9项，右侧的菜单项也有9项，所以左侧菜单项的索引为0~8，右侧菜单项的索引为9~17。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_4.png)

* 功能：鼠标经过某个元素，当前元素样式和内容可以展现出来，其他的兄弟节点的样式和内容隐藏。

* 重点：`mouseenter()、index()、show()、eq(index)、siblings()、hide()`

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
            font-size: 12px;
        }
        ul {
            list-style: none;
        }
        img {
            border: 0;
        }
        a {
            text-decoration: none;
        }
        .wrapper {
            width: 298px;
            height: 248px;
            margin: 100px auto 0;
            border: 1px solid pink;
            overflow: hidden;
        }
        #left,#center,#right{
            float: left;
        }
        #left li , #right li{
            background: url(images/lili.jpg) repeat-x;
        }
        #left li a,#right li a{
            display: block;
            width: 48px;
            height: 27px;
            border-bottom: 1px solid pink;
            line-height: 27px;
            text-align: center;
            color: black;
        }
        #left li a:hover,#right li a:hover{
            background-image: url(images/abg.gif);
        }
        #center {
            border-left: 1px solid pink;
            border-right: 1px solid pink;
        }
    </style>
    <script src="jquery-1.11.1.min.js"></script>
   <script type="text/javascript">
        $(document).ready(function(){
            //上个案例已经讲了mouseover和mouseenter区别，大家一定要认真的看下。

            // 1. 给左边菜单绑定鼠标进入事件 mouseenter
            $("#left > li").mouseenter(function () {
                var index = $(this).index(); // 获取当前元素的序号

                // 根据序号选择到要展示的元素，让它展示出来，并且让它所有的兄弟元素都隐藏掉
                $("#center > li").eq(index).show();
                $("#center > li").eq(index).siblings().hide();
            });

            // 2. 给右边菜单绑定鼠标进入事件 mouseenter
            $("#right > li").mouseenter(function () {
                var index = $(this).index(); // 获取当前元素的索引号

                // 由于当前序号跟中间广告的序号存在一个数量差的关系，所以，这个地方我们应该给
                // 所有的序号 + 9
                $("#center > li").eq(index+9).show();
                $("#center > li").eq(index+9).siblings().hide();
            });
        });
    </script>
</head>
<body>
<div class="wrapper">

    <ul id="left">
        <li><a href="#">女靴</a></li>
        <li><a href="#">雪地靴</a></li>
        <li><a href="#">冬裙</a></li>
        <li><a href="#">呢大衣</a></li>
        <li><a href="#">毛衣</a></li>
        <li><a href="#">棉服</a></li>
        <li><a href="#">女裤</a></li>
        <li><a href="#">羽绒服</a></li>
        <li><a href="#">牛仔裤</a></li>
    </ul>
    <ul id="center">
        <li><a href="#"><img src="images/女靴.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/雪地靴.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/冬裙.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/呢大衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/毛衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/棉服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/女裤.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/羽绒服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/牛仔裤.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/女包.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男包.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/登山鞋.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/皮带.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/围巾.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/皮衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男毛衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男棉服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男靴.jpg" width="200" height="250" /></a></li>
    </ul>
    <ul id="right">
        <li><a href="#">女包</a></li>
        <li><a href="#">男包</a></li>
        <li><a href="#">登山鞋</a></li>
        <li><a href="#">皮带</a></li>
        <li><a href="#">围巾</a></li>
        <li><a href="#">皮衣</a></li>
        <li><a href="#">男毛衣</a></li>
        <li><a href="#">男棉服</a></li>
        <li><a href="#">男靴</a></li>
    </ul>
</div>
</body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_4.gif)

## 5.表格全选反选效果

* 功能：当点击全选的复选框时，所有的子复选框都会被选中，当点击全部选的复选框时，所有的子复选框都不被选中，当所有的子复选框被选中是，全选的复选框自动被选中。

* 重点：Jquery对象转换成DOM对象、然后使用DOM对象的innerHTML()方法、(":checkbox")、(":checked")、DOM对象中的checked()、三元表达式。

```javascript
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <title></title>
        <style>
            * {
                padding: 0;
                margin: 0;
            }
            
            .wrap {
                width: 300px;
                margin: 100px auto 0;
            }
            
            table {
                border-collapse: collapse;
                border-spacing: 0;
                border: 1px solid #c0c0c0;
            }
            th,
            td {
                border: 1px solid #d0d0d0;
                color: #404060;
                padding: 10px;
            }
            
            th {
                background-color: #09c;
                font: bold 16px "微软雅黑";
                color: #fff;
            }
            
            td {
                font: 14px "微软雅黑";
            }
            
            tbody tr {
                background-color: #f0f0f0;
            }
            
            tbody tr:hover {
                cursor: pointer;
                background-color: #fafafa;
            }
        </style>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script>
            $(function() {
                var $j_cbAll = $("#j_cbAll"), // 获取全选checkbox：jQuery对象
                    j_cbAll = $j_cbAll[0], // 获取全选checkbox：DOM对象
                    $tb = $("#j_tb"), // 获取tbody
                    $cbs = $tb.find(":checkbox"), // 获取tbody中所有的复选框
                    cbsLen = $cbs.length; // 获取复选框的长度

                // 给全选checkbox绑定单击事件：处理所有选项的checkbox选中状态
                $j_cbAll.click(function() {
                    var i = 0; // 注意此处i的使用

                    // 全选
                    // if(this.checked) 可以吗？
                    if(this.checked === true) {
                        for(; i < cbsLen; i++) {
                            // 设置复选框被选中
                            $cbs[i].checked = true;
                        }
                    } else {
                        // 全不选
                        for(; i < cbsLen; i++) {
                            // 设置复选框不被选中
                            $cbs[i].checked = false;
                        }
                    }
                });

                // 给所有 tbody中的 checkbox元素 绑定click事件
                $cbs.click(function() {
                    // 获取所有被选中的checkbox个数
                    var $selCbLen = $tb.find(":checkbox:checked"); // 此处只有复选框
                    // 判断是否选中 全选checkbox
                    /*if(selCbLen.length === cbsLen) {
                        j_cbAll.checked = true;
                    } else {
                        j_cbAll.checked = false;
                    }*/

                    $selCbLen.length === cbsLen ?
                        j_cbAll.checked = true :
                        j_cbAll.checked = false;
                });
            });
        </script>
    </head>

    <body>
        <div class="wrap">
            <table>
                <thead>
                    <tr>
                        <th><input type="checkbox" id="j_cbAll" /></th>
                        <th>课程名称</th>
                        <th>所属学院</th>
                    </tr>
                </thead>
                <tbody id="j_tb">
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>JavaScript</td>
                        <td>前端与移动开发学院</td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>css</td>
                        <td>前端与移动开发学院</td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>html</td>
                        <td>前端与移动开发学院</td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>jQuery</td>
                        <td>前端与移动开发学院</td>
                    </tr>

                </tbody>
            </table> 
        </div>
   </body>
</html>

```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_5.gif)

注意：for循环和for in循环的区别

for/in 循环会访问该对象的原型，应该用在非数组对象的遍历上，for循环应该遍历数组。

## 6.省选择案例

* 功能：全部选择和单个选择

* 重点：Jquery对象转换成DOM对象，然后使用DOM对象的innerHTML()、outerHTML()、removeClass()、(":selected")、children()。

```javascript
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        select {
            width: 200px;
            background-color: teal;
            height: 200px;
            font-size: 20px;
        }
        .btn-box {
            width: 30px;
            display: inline-block;
            vertical-align: top;
        }
    </style>
    <script src="jquery-1.11.1.min.js"></script>
    <script>
        $(function () {
            var $srcCity = $("#src-city"), // 获取到 左边（源） 选择框：jQuery对象
                $tarCity = $("#tar-city"), // 获取到 右边（目标） 选择框：jQuery对象
                srcCity = $srcCity[0], // 左边（源） 选择框：DOM对象
                tarCity = $tarCity[0]; // 右边（目标） 选择框：DOM对象
            // >> 操作
            $("#btn-sel-all").click(function () {
                // 把 源选择框 中的数据 添加到 目标选择框 中
                tarCity.innerHTML += srcCity.innerHTML;
                // 清空源 所有数据
                srcCity.innerHTML = "";
            });
            // << 操作
            $("#btn-sel-none").click(function () {
                // 把 目标选择框 中的数据 添加到 源选择框 中
                srcCity.innerHTML += tarCity.innerHTML;
                // 清空目标 所有数据
                tarCity.innerHTML = "";
            });

            // > 操作 （不支持多选）
            $("#btn-sel").click(function () {
                // 获取到当前选中的option
                var selCitySel = $srcCity.children(":selected")[0];
                // 把 源选择框 选中的数据 添加到 目标选择框 中
                tarCity.innerHTML += selCitySel.outerHTML;
                // 移除 源中的选中元素
                srcCity.removeChild(selCitySel);
            });

            // < 操作 （不支持多选）
            $("#btn-back").click(function () {
                // 获取到当前选中的option
                var tarCitySel = $tarCity.children(":selected")[0];
                // 把 目标选择框 选中的数据 添加到 源选择框 中
                srcCity.innerHTML += tarCitySel.outerHTML;
                // 移除 目标中的选中元素
                tarCity.removeChild(tarCitySel);
            });
        });
    </script>
</head>
<body>
    <h1>城市选择：</h1>
    <select id="src-city" name="src-city" multiple>
        <option value="1">北京</option>
        <option value="2">上海</option>
        <option value="3">深圳</option>
        <option value="4">广州</option>
        <option value="5">西红柿</option>
    </select>
    <div class="btn-box">
        <button id="btn-sel-all"> &gt;&gt; </button>
        <button id="btn-sel-none"> &lt;&lt; </button>
        <button id="btn-sel"> &gt;</button>
        <button id="btn-back"> &lt; </button>
    </div>
    <select id="tar-city" name="tar-city" multiple>
    </select>
</body>
</html>
```
注意："+="表示一个累加的过程，"+"表示一个迭代覆盖的过程

运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_6.gif)

## 7.下拉菜单

* 功能：当鼠标进入出现下拉菜单、当鼠标移除隐藏下拉菜单

* 重点：mouseover()、mouseenter()函数、mouseleave()、children()函数、show()、hide()、css()

不论鼠标指针穿过被选元素或其子元素，都会触发mouseover事件

只有在鼠标指针穿过被选元素时，才会触发mouseenter事件

```javascript
<!-- demo1.html -->
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css">
            * {
                margin: 0;
                padding: 0;
            }
            ul {
                list-style: none;
            }
            .wrap {
                width: 330px;
                height: 30px;
                margin: 100px auto 0;
                padding-left: 10px;
                background-image: url(imgs/bg.jpg);
            }
            .wrap li {
                background-image: url(imgs/libg.jpg);
            }
            .wrap>ul>li {
                float: left;
                margin-right: 10px;
                position: relative;
            }
            .wrap a {
                display: block;
                height: 30px;
                width: 100px;
                text-decoration: none;
                color: #000;
                line-height: 30px;
                text-align: center;
            }
            .wrap li ul {
                position: absolute;
                top: 30px;
                display: none;
            }
        </style>
        <script src="../js/jquery.min.js"></script>
         <script type="text/javascript">
            $(document).ready(function() {

                // 1. 获取到事件源,并绑定鼠标进入事件
                $(".wrap > ul > li").mouseenter(function() {
                    // 让当前一级菜单下面的二级菜单展示出来
                    $(this).children("ul").show();
                    $(this).children("ul").css("display","block");  
                });
                // 2. 获取到事件源，并绑定鼠标离开事件
                $(".wrap > ul > li").mouseleave(function() {
                    // 让当前一级菜单下面的二级菜单隐藏掉
                    $(this).children("ul").hide(); // hide() 方法表示：隐藏
                    $(this).children("ul").css("display","none");   
                });
            });
        </script>
    </head>
    <body>
        <div class="wrap">
            <ul>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </body>
</html>
```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_7.gif)

## 8.动画效果的下拉菜单

* 功能：当鼠标进入出现卷帘门式的出现下拉菜单、当鼠标移除的时候卷帘门式的隐藏下拉菜单

* 重点：mouseover()、mouseleave、mouseenter()函数、children()函数、css()、slideDown()、slideUp()、hover()、toggle()、stop()。

**第一种方式**

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <style type="text/css">
            * {
                margin: 0;
                padding: 0;
            }
            
            ul {
                list-style: none;
            }
            
            .wrap {
                width: 330px;
                height: 30px;
                margin: 100px auto 0;
                padding-left: 10px;
                background-image: url(imgs/bg.jpg);
            }
            
            .wrap li {
                background-image: url(imgs/libg.jpg);
            }
            
            .wrap>ul>li {
                float: left;
                margin-right: 10px;
                position: relative;
            }
            
            .wrap a {
                display: block;
                height: 30px;
                width: 100px;
                text-decoration: none;
                color: #000;
                line-height: 30px;
                text-align: center;
            }
            
            .wrap li ul {
                position: absolute;
                top: 30px;
                display: none;
            }
        </style>
        <script src="../js/jquery.min.js"></script>
         <script type="text/javascript">
            $(document).ready(function() {
                // 1. 获取到事件源,并绑定鼠标进入事件
                $(".wrap > ul > li").mouseenter(function() {
                    // 让当前一级菜单下面的二级菜单展示出来
                    $(this).children("ul").slideDown();
                    $(this).children("ul").css("display","block");
                });
                // 2. 获取到事件源，并绑定鼠标离开事件
                $(".wrap > ul > li").mouseleave(function() {
                    // 让当前一级菜单下面的二级菜单隐藏掉
                    $(this).children("ul").slideUp(); // hide() 方法表示：隐藏
                    $(this).children("ul").css("display","none");
                });
            });
        </script>
    </head>
    <body>
        <div class="wrap">
            <ul>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
                <li>
                    <a href="javascript:void(0);">一级菜单1</a>
                    <ul>
                        <li>
                            <a href="javascript:void(0);">二级菜单1</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单2</a>
                        </li>
                        <li>
                            <a href="javascript:void(0);">二级菜单3</a>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </body>

</html>

```
**第二种方式：**

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

**第三种方式：**

```javascript
 <script type="text/javascript">
  $(document).ready(function(){
                $(".wrap > ul > li").hover(function(){
                     $(this).children("ul").slideToggle();
                })
            })
</script>
```
**第四种方式**

`stop()`：`slideToggle()`动画的效果是切换卷帘门，当鼠标进入或者离开的时候会触发切换效果，该动画默认有一定的执行时间，只有当时间间隔到达时才会停止动画，并不是说鼠标离开就会停止动画，利用`stop()`方法可以立即停止动画，没有时间间隔，这样的效果更贴近现实情况一些。

```javascript
 <script type="text/javascript">
  $(document).ready(function(){
                $(".wrap > ul > li").hover(function(){
                     $(this).children("ul").stop().slideToggle();
                })
            })
</script>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_8.gif)

## 9右侧的QQ交谈

* 功能：点击在线客服就会出现QQ在线客服的界面

* 重点：animate()、children()、attr()

```javascript
<!DOCTYPE HTML>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <title>无标题文档</title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
                list-style: none;
                border: 0;
            }
            .all {
                width: 131px;
                height: 311px;
                position: fixed;
                right: -131px;
                top: 50%;
                margin-top: -155px;
            }
            
            .all span {
                position: absolute;
                left: -29px;
                top: 50%;
                margin-top: -58px;
                cursor: pointer;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(function() {
                var bl = true;
                $('.all span').click(function(e) {
                    //如果能判断是折叠还是展开即可；
                    if(bl === true) {

                        $('.all').animate({
                            right: 0
                        });
                        $(this).children().attr('src', 'imgs/qqLOpen.jpg');
                        bl = false;
                    } else {

                        $('.all').animate({
                            right: -131
                        });
                        $(this).children().attr('src', 'imgs/qqL.jpg');
                        bl = true;
                    }
                });
            })
        </script>
    </head>
    <body>
        <div class="all">
            <span><img src="imgs/qqL.jpg" width="29" height="117"></span>
            <img src="imgs/qq.jpg" width="131" height="311">
        </div>

    </body>
</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_9.gif)

## 10.上下焦点图的制作

* 功能：当鼠标每经过一个小图标时，图片会自动上下转换，当鼠标不在图片上时，图片会自动上下转换。

* 重点：mouseenter()、addClass()、siblings()、removeClass()、index()、stop()、animate()、setInterval()、addClass()、hover()、clearInterval()

```javascript
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
                list-style: none;
                border: 0;
            }
            .all {
                width: 490px;
                height: 170px;
                margin: 100px auto;
                position: relative;
                padding: 5px;
                border: 1px solid #ccc;
            }
            .all div {
                width: 490px;
                height: 170px;
                position: relative;
                overflow: hidden;
            }
            
            .all ul {
                position: absolute;
                left: 0;
                top: 0;
            }
            
            .all img {
                display: block;
            }
            
            .all ol {
                position: absolute;
                right: 10px;
                bottom: 10px;
            }
            
            .all ol li {
                width: 20px;
                height: 20px;
                background: #fff;
                border: 1px solid #ccc;
                float: left;
                margin-left: 10px;
                line-height: 20px;
                text-align: center;
                cursor: pointer;
            }
            
            .all ol li.current {
                background: yellow;
            }
        </style>

        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {
                $(".all ol li").mouseenter(function() {
                    $(this).addClass("current").siblings().removeClass("current");
                    var index = $(this).index();
                    $("ul ").stop().animate({
                        top: index * -170
                    });
                    num = index; //index()获取当前元素的索引号
                });
                //制作一个定时器去实现自动播放功能； 定时器模块
                var timer = null;
                var num = 0;
                timer = setInterval(autoplay, 2000);
                function autoplay() {
                    num++;
                    if(num > 4) {
                        num = 0;
                    }
                    $('.all ol li').eq(num).addClass('current').siblings().removeClass();
                    $('.all ul').stop().animate({
                        top: num * -170
                    };
                }
                //鼠标移上停止定时器模块，hover切换动画
                $('.all div').hover(function(e) {
                    //停止自动播放，清除定时器；
                    clearInterval(timer);
                }, function(e) {
                    clearInterval(timer);
                    timer = setInterval(autoplay, 2000);
                });

            });
        </script>
        </script>
    </head>
    <body>
        <div class="all">
            <div>
                <ul>
                    <li><img src="imgs/01.jpg" width="490" height="170"></li>
                    <li><img src="imgs/02.jpg" width="490" height="170"></li>
                    <li><img src="imgs/03.jpg" width="490" height="170"></li>
                    <li><img src="imgs/04.jpg" width="490" height="170"></li>
                    <li><img src="imgs/05.jpg" width="490" height="170"></li>
                </ul>
                <ol>
                    <li class="current">1</li>
                    <li>2</li>
                    <li>3</li>
                    <li>4</li>
                    <li>5</li>
                </ol>
            </div>
        </div>
    </body>
</html>

```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_10.gif)

## 11.左右焦点图的制作

* 功能：当鼠标每经过一个小图标时，图片会自动左右转换，当鼠标不在图片上时，图片会自动左右转换。

* 重点：mouseenter()、addClass()、siblings()、removeClass()、index()、stop()、animate()、setInterval()、addClass()、hover()、clearInterval()

```
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
                list-style: none;
                border: 0;
            }
            
            .all {
                width: 400px;
                height: 307px;
                border: 5px solid #ccc;
                margin: 100px auto;
                position: relative;
            }
            
            .all .show {
                position: relative;
                overflow: hidden;
                width: 400px;
                height: 307px;
            }
            
            .all ul {
                width: 9999px;
                height: 307px;
                position: absolute;
                left: 0;
                top: 0;
            }
            
            .all ul li {
                float: left;
            }
            
            .all span {
                width: 52px;
                height: 52px;
                background: url(imgs/arr.png);
                position: absolute;
                top: 50%;
                margin-top: -26px;
                left: -21px;
                cursor: pointer;
            }
            
            .all .right {
                width: 52px;
                height: 52px;
                background: url(imgs/arr.png) no-repeat right 0;
                position: absolute;
                top: 50%;
                margin-top: -26px;
                right: -21px;
                left: auto;
            }
            
            .all ol {
                position: absolute;
                bottom: -30px;
                left: 30%;
                cursor: pointer;
            }
            
            .all ol li {
                width: 13px;
                height: 13px;
                background: url(imgs/00.png) no-repeat 0 0;
                float: left;
                margin-left: 10px;
                font-size: 0;
            }
            
            .all ol .current {
                background-position: 0 bottom;
            }
            
            .box li {
                background: red;
                border: 1px solid #000;
            }
        </style>

        <script src="../js/jquery.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {
                var sx = 0
                var timer = null;
                //var num=0;

                $('.all ol li').click(function(e) {
                    var index = $(this).index();
                    $(this).addClass('current').siblings().removeClass();

                    $('.all ul').stop().animate({
                        left: index * -400
                    });

                    sx = index;
                });

                //箭头工作
                $('.all .right').click(function(e) {
                    sx++;
                    if(sx > 5) {
                        sx = 0;
                    }
                    //都有谁需要跟着这个顺序走。
                    $('.all ol li').eq(sx).addClass('current').siblings().removeClass();

                    $('.all ul').stop().animate({
                        left: sx * -400
                    });
                });

                $('.all .left').click(function(e) {
                    sx--;
                    if(sx < 0) {
                        sx = 5;
                    }
                    //都有谁需要跟着这个顺序走。
                    $('.all ol li').eq(sx).addClass('current').siblings().removeClass();

                    $('.all ul').stop().animate({
                        left: sx * -400
                    });
                });
                //自动播放模块
                //制作一个定时器去实现自动播放功能； 定时器模块
                timer = setInterval(autoplay, 2000);
                function autoplay() {
                    sx++;
                    if(sx > 5) {
                        sx = 0;
                    }
                    $('.all ol li').eq(sx).addClass('current').siblings().removeClass(); //表示的是角标
                    $('.all ul').stop().animate({
                        left: sx * -400
                    });
                }
                //鼠标移上停止定时器模块
                $('.all').hover(function(e) {
                    //停止自动播放，清除定时器；
                    clearInterval(timer);
                }, function(e) {
                    clearInterval(timer);
                    timer = setInterval(autoplay, 2000);
                });
            });
        </script>
    </head>
    <body>
        <body>
            <div class="all">
                <div class="show">
                    <ul>
                        <li><img src="imgs/001.jpg" width="400" height="307"></li>
                        <li><img src="imgs/002.jpg" width="400" height="307"></li>
                        <li><img src="imgs/003.jpg" width="400" height="307"></li>
                        <li><img src="imgs/004.jpg" width="400" height="307"></li>
                        <li><img src="imgs/005.jpg" width="400" height="307"></li>
                        <li><img src="imgs/006.jpg" width="400" height="307"></li>
                    </ul>
                </div>
                <span class="left"></span>
                <span class="right"></span>
                <ol>
                    <li class="current"></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                </ol>
            </div>
        </body>
    </body>
</html>

```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_11.gif)


## 12.表格全选、反选增强版

* 功能：当点击全选的复选框时，所有的子复选框都会被选中，当点击全部选的复选框时，所有的子复选框都不被选中，当所有的子复选框被选中是，全选的复选框自动被选中，所有的选中与不选中都包括新创建的节点。当点击添加袁术时，会弹出新的对话框，当点击get时，会删除这一项，或者同时删除好几项。

* 重点：(":checkbox")、find()、prop()、attr()、(:checked")、三元表达式、parent()、remove(); show()、val()、append()、hide()

```javascript
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            * {
                padding: 0;
                margin: 0;
            }
            
            .wrap {
                width: 410px;
                margin: 100px auto 0;
            }
            
            table {
                border-collapse: collapse;
                border-spacing: 0;
                border: 1px solid #c0c0c0;
            }
            
            th,
            td {
                border: 1px solid #d0d0d0;
                color: #404060;
                padding: 10px;
            }
            
            th {
                background-color: #09c;
                font: bold 16px "微软雅黑";
                color: #fff;
            }
            
            td {
                font: 14px "微软雅黑";
            }
            
            td a.get {
                text-decoration: none;
            }
            
            a.del:hover {
                text-decoration: underline;
            }
            
            tbody tr {
                background-color: #f0f0f0;
            }
            
            tbody tr:hover {
                cursor: pointer;
                background-color: #fafafa;
            }
            
            .form-item {
                height: 100%;
                position: relative;
                padding-left: 100px;
                padding-right: 20px;
                margin-bottom: 34px;
                line-height: 36px;
            }
            
            .form-item>.lb {
                position: absolute;
                left: 0;
                top: 0;
                display: block;
                width: 100px;
                text-align: right;
            }
            
            .form-item>.txt {
                width: 300px;
                height: 32px;
            }
            
            .mask {
                position: absolute;
                top: 0px;
                left: 0px;
                width: 100%;
                height: 100%;
                background: #000;
                opacity: 0.15;
                display: none;
            }
            
            .form-add {
                position: fixed;
                top: 30%;
                left: 50%;
                margin-left: -197px;
                padding-bottom: 20px;
                background: #fff;
                display: none;
            }
            
            .form-add-title {
                background-color: #f7f7f7;
                border-width: 1px 1px 0 1px;
                border-bottom: 0;
                margin-bottom: 15px;
                position: relative;
            }
            
            .form-add-title span {
                width: auto;
                height: 18px;
                font-size: 16px;
                font-family: 宋体;
                font-weight: bold;
                color: rgb(102, 102, 102);
                text-indent: 12px;
                padding: 8px 0px 10px;
                margin-right: 10px;
                display: block;
                overflow: hidden;
                text-align: left;
            }
            
            .form-add-title div {
                width: 16px;
                height: 20px;
                position: absolute;
                right: 10px;
                top: 6px;
                font-size: 30px;
                line-height: 16px;
                cursor: pointer;
            }
            
            .form-submit {
                text-align: center;
            }
            
            .form-submit input {
                width: 170px;
                height: 32px;
            }
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(document).ready(function() {
                var $j_cbAll = $("#j_cbAll"), // 获取全选checkbox：jQuery对象
                    j_cbAll = $j_cbAll[0], // 获取全选checkbox：DOM对象
                    $tb = $("#j_tb"), // 获取tbody
                    $cbs = $tb.find(":checkbox"), // 获取tbody中所有的复选框
                    cbsLen = $cbs.length; // 获取复选框的长度
                // 给全选checkbox绑定单击事件：处理所有选项的checkbox选中状态
                $j_cbAll.click(function() {
                    var i = 0; // 注意此处i的使用
                    // 全选
                    // if(this.checked) 可以吗？
                    $cbs = $tb.find(":checkbox"); // 获取tbody中所有的复选框
                    cbsLen = $cbs.length;
                    if($(this).prop("checked") === true) {
                        for(; i < cbsLen; i++) {
                            // 设置复选框被选中(第一种方式是DOM对象方式、第二种方式Jquery对象，只能选择一次、第三种方式是Jquery对象、复选框可以选择多次)
                            //$cbs[i].checked = true;
                            //$cbs.attr("checked",true);
                            $cbs.prop("checked", true);
                        }
                    } else {
                        // 全不选
                        for(; i < cbsLen; i++) {
                            // 设置复选框不被选中(第一种方式是DOM对象方式、第二种方式Jquery对象，只能选择一次、第三种方式是Jquery对象、复选框可以选择多次)
                            //$cbs[i].checked = false;
                            //$cbs.attr("checked",false);
                            $cbs.prop("checked", false);
                        }
                    }
                });
                // 给所有 tbody中的 checkbox元素 绑定click事件
                // 给所有 tbody中的 checkbox元素 绑定click事件
                $cbs.click(function() {
                    $cbs = $tb.find(":checkbox"); // 获取tbody中所有的复选框
                    cbsLen = $cbs.length;
                    // 获取所有被选中的checkbox个数
                    var $selCbLen = $tb.find(":checkbox:checked"); // 此处只有复选框
                    // 判断是否选中 全选checkbox
                    /*if(selCbLen.length === cbsLen) {
                        j_cbAll.checked = true;
                    } else {
                        j_cbAll.checked = false;
                    }*/

                    $selCbLen.length === cbsLen ?
                        j_cbAll.checked = true :
                        j_cbAll.checked = false;
                });

                //get
                $(".get").click(function() {
                    //选择到当前这个a标签的父元素td，在获取到td的父元素tr，让tr自己remove掉
                    $(this).parent().parent().remove();
                })

                //get选中按钮
                $("#j_btnDelSel").click(function() {
                    //获取到选中的复选框的个数
                    var checkedLen = $tb.find(":checkbox:checked").length;
                    if(checkedLen <= 0) {
                        alert("请选择要get的技能");
                        return;
                    }
                    //选择到所有的被选中的checkbox,
                    $tb.find(":checkbox:checked").parent("td").parent("tr").remove();
                    $j_cbAll.prop("checked", false)
                })

                //添加按钮展示 添加表单层
                $("#j_btnAddData").click(function() {
                    $("#j_mask").show();
                    $("#j_formAdd").show();
                });
                //添加数据功能
                $("#j_btnAdd").click(function() {
                    //因为用的是DOM对象中的方法，所以要jQuery对象要转化成DOM对象
                    //获取到课程的值(Val()函数，获取文本框的值)
                    var lessonvalue = $("#j_txtLesson").val();
                    //获取到所属学院的值
                    var BelSch = $("#j_txtBelSch").val();
                    var trHml =
                        "<tr>" +
                        "<td><input type='checkbox' /></td>" +
                        "<td>" + lessonvalue + "</td>" +
                        "<td>" + BelSch + "</td>" +
                        "<td><a href='javascrip:;' class='get'>GET</a></td>" +
                        "</tr>"
                    //tr、td是tbody的子元素
                    $tb.append(trHml); //直接添加到所指定元素的所有子元素的最后面
                    $("#j_mask").hide();
                    $("#j_formAdd").hide();
                });
            });
        </script>
    </head>

    <body>
        <div class="wrap">
            <div>
                <input type="button" value="GET选中" id="j_btnDelSel" />
                <input type="button" value="添加数据" id="j_btnAddData" />
            </div>
            <table>
                <thead>
                    <tr>
                        <th><input type="checkbox" id="j_cbAll" /></th>
                        <th>课程名称</th>
                        <th>所属学院</th>
                        <th>已学会</th>
                    </tr>
                </thead>
                <tbody id="j_tb">
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>JavaScript</td>
                        <td>传智播客-前端与移动开发学院</td>
                        <td>
                            <a href="javascrip:;" class="get">GET</a>
                        </td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>css</td>
                        <td>传智播客-前端与移动开发学院</td>
                        <td>
                            <a href="javascrip:;" class="get">GET</a>
                        </td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>html</td>
                        <td>传智播客-前端与移动开发学院</td>
                        <td>
                            <a href="javascrip:;" class="get">GET</a>
                        </td>
                    </tr>
                    <tr>
                        <td><input type="checkbox" /></td>
                        <td>jQuery</td>
                        <td>传智播客-前端与移动开发学院</td>
                        <td>
                            <a href="javascrip:;" class="get">GET</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div id="j_mask" class="mask"></div>
        <div id="j_formAdd" class="form-add">
            <div class="form-add-title">
                <span>添加数据</span>
                <div id="j_hideFormAdd">x</div>
            </div>
            <div class="form-item">
                <label class="lb" for="j_txtLesson">课程名称：</label>
                <input class="txt" type="text" id="j_txtLesson" placeholder="请输入课程名称">
            </div>
            <div class="form-item">
                <label class="lb" for="j_txtBelSch">所属学院：</label>
                <input class="txt" type="text" id="j_txtBelSch" value="传智播客-前端与移动开发学院">
            </div>
            <div class="form-submit">
                <input type="button" value="添加" id="j_btnAdd">
            </div>
        </div>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_12.gif)

## 13.右下角弹出广告

* 功能：右下角的小广告先展示再隐藏后显示，同时可以关闭广告

* 重点：slideDown()、slideUp()、fadeIn()、parent()、fadeOut()

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
             .ad {
            position: fixed;
            right: 0;
            bottom: 0;
            width: 230px;
            height: 120px;
            background-image: url(imgs/ad.jpg);
            display: none;
        }
        .ad span {
            position: absolute;
            right: 0;
            top: 0;
            width: 40px;
            height: 18px;
            background-image:url(imgs/h.jpg);
        </style>
        <script type="text/javascript" src="../js/jquery-1.11.1.min.js" ></script>
        <script type="text/javascript">
            $(document).ready(function(){
                $('.ad').slideDown(1000).slideUp(1000).fadeIn();
                $('span').click(function(){
                    $(this).parent().fadeOut();
                }); 
            })  
        </script>
    </head>
    <body>
        <div class="ad">
            <span></span>
        </div>
    </body>
</html>
```
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_13.gif)

## 14.鼠标跟随效果

* 功能：图片会跟随着鼠标的移动而变化着

* 重点：mousemove()、offset()、pageX、pageY、监听整个document的鼠标移动事件

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            body{
                background-color: antiquewhite;
            }
        </style>
        <script src="../js/jquery-1.11.1.min.js"></script>
        <script type="text/javascript">
            $(function(){
                $(document).on("mousemove",function(e){
                    $("img").offset({
                        top: e.pageY,
                        left:e.pageX
                    })
                })
            })
        </script>
    </head>
    <body>
        <img src="imgs/ts.gif"/>
    </body>
</html>
```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_14.gif)

## 15.五角星评论案例

* 功能:当鼠标移入时，自己和自己前面的五角星都变成实心，自己后面的五角星是空心，当鼠标离开时，实心显示到被点击的五角星上。

* 重点:text()、prevAll()、end()、nextAll()、addClass()、siblings()、removeClass()、(".clicked")、mouseleave()、mouseenter()

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>五角星评论案例</title>
    <style>
        * {
            padding: 0;
            margin: 0;
          }
        .comment {
            font-size: 40px;
            color: teal;
        }
        .comment li {
            float: left;
        }
        ul {
            list-style: none;
        }
    </style>
    <script src="jquery-1.11.1.min.js"></script>
    <script>
        $(function(){
            var wjx_none = '☆',
                wjx_sel  = '★';
            $(".comment li").mouseenter(function(){
                //鼠标移入: 自己和前面的兄弟变实心，其余变空心
                $(this).text(wjx_sel).prevAll().text(wjx_sel).end().nextAll().text(wjx_none);
            }).click(function(){
                //鼠标点击后，把自己添加clicked类，其余的清除clicked类
                $(this).addClass('clicked').siblings().removeClass('clicked');
            });

            //当鼠标移开评分控件时，实心显示到被点击的五角星的上
            $(".comment").mouseleave(function(){
                $(".comment li").text(wjx_none);//先给所有五角星都变空心
                $(".clicked").text(wjx_sel).prevAll().text(wjx_sel).end().nextAll().text(wjx_none);
            });
        });
    </script>
</head>
<body>
    <ul class="comment">
        <li>☆</li>
        <li>☆</li>
        <li>☆</li>
        <li>☆</li>
        <li>☆</li>
    </ul>
</body>
</html>

```

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232446/o_15.gif)
