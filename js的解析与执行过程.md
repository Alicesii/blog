---
title: js的解析与执行过程
comments: true
date: 2018-06-02 09:52:37
categories: 博客列表
tags: 预解析过程

---

# 1.全局预处理与执行

## 1.1.预处理

1.1.1 引例：

第一种情况

```javaScript
<script type="text/javascript">
var a=1;
function xx(){
	console.log(a);
}
xx();
</script>

```

运行结果：在控制台输出1

原因：在`xx()`函数中可以访问一个全局的变量a,而现在这个全局变量a的值为1,所以输出结果为1。

第二种情况：

```javaScript
<script type="text/javascript">
var a=1;
function xx(){
	console.log(a);
	var a=5;
}
xx();
</script>

```

运行结果：在控制台输出undefind

原因：在我们看来输出结果绝对不可能是5,因为浏览器引擎都是按照程序的顺序渲染程序的，那为什么会输出undefind？首先我们需要注意的是该代码在运行的时候并没有报错，而我们一般使用不存在的变量的时候，浏览器都会报错,但是现在不报错，值却是undefined，这个现象就是js内部的解析机制在捣鬼。现在我们就一起来了解一下JS的解析机制。

1.1.2 预处理阶段

JS的解析与执行过程主要分为两个阶段：预处理的阶段和执行阶段

注意：浏览器在解析JS代码的时候，并不是读一行就处理一行代码，它分为两块，第一块就是在正式处理代码之前的预处理阶段
，第二块就是执行阶段，现在我人为的将它分为两部分：全局部分和函数部分，如下图所示：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_0.png)

在预处理阶段分为两部分：

第一部分：创建一个词法环境对象

第二部分：扫描你整个JS全局代码中的两个部分

* 用声明的方式创建的函数

* 用var定义的变量

```JavaScript

//词法环境对象
LexicalEnvironment{

}

```
扫描出来以后就把这些变量的名字、函数的名字加到全局的词法环境对象中去。

举例：

```javascript
<script type="text/javascript">
var a=5;
var b;
c=5;  //没有var，所以不能加到词法环境中
function xxx(){

}
</script>
```
词法环境对象：

```JavaScript
LexicalEnvironment {
   a:undefined
   b:undefined
   xxx:对函数的一个引用  
}
```

注意：一定要用声明方式的创建函数，例如像这样`var g=function()`这样的方式叫做函数表达式，而不是声明的方式。

举例：

```javascript
<script type="text/javascript">
f();
g();
function f(){     //以声明的方式创建的函数
	console.log('ff');
}
var g=function(){  //以函数表达式创建的函数
	console.log('gg');
}
</script>

```

运行结果：
![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_1.png)

原因：f函数会被正常的调用，函数f在词法环境中，g函数的调用会报错，在浏览器在执行过程中，在预处理阶段，将f函数写在词法环境中，是对函数的一个引用，f->ff，在g函数在词法环境中根本不存在，这里的g代表的是一个普通的变量，在预处理阶段的值为undefined,而不是一个函数，所以调用的时候会报错。

```javascript
<script type="text/javascript">

console.log(a);
console.log(b)
var a=5;
b=6;
</script>

```

运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_2.png)

原因我就不细说了，相信大家通过上面的例子已经看明白了吧。


总结：说白了预处理阶段就是分为两种情况

* 用var声明的变量的时候，变量是存在的，但值为undefined

* 用声明的方式声明的函数的时候，保持着对函数的引用。


1.1.3 全局的词法环境对象就window对象： `LexicalEnvironment===window`

## 1.2.命名冲突问题

1.2.1 引例：

```javascript
<script type="text/javascript">
var f=5;
fucntion f(){}

var f=6

function f(){
	
}
</script>

```
面对这种函数和变量同名的情况，词法环境对象(window对象)会怎么处理呢？

```javascript

window{

f:可能出现出现两次   //在JS中是不可能存在的情况，不可能一个名称既代表函数名又代表变量名
f:出现一次的策略 
} 
```
在预处理阶段是JS引擎自己解析的，人为是无法参与的进去的，所以所有的预处理过程是在代码执行之前工作的。

```javascript
<script type="text/javascript">
alert(f);
var f=5;
function f(){
	console.log('111');
}
</script>


```


```javascript

<script type="text/javascript">
console.log(f);
function f(){
	console.log('111');
}

var f=5;
</script>


```
运行结果:

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_3.png)

原因：并不是因为以f命名的函数在靠后的位置，输出结果为函数f的字符串表示，调整变量和函数的位置，输出结果还是相同的，所以并不是简单的顺序的扫描程序，若发生重名冲突的话，以后面的覆盖前面的，而真正的原因是因为JS引擎解决变量和函数的冲突机制。现在我们来一起了解一下吧。

1.2.2 命名冲突策略：

* 先扫描函数声明后扫描var声明的变量

* 处理函数声明有冲突，会覆盖

* 处理变量声明有冲突，会忽略

注意：是函数声明的方式而不是函数表达式的方式，我一直在强调这一点。

所以上述代码的词法环境为(处理变量和函数冲突)

```javascript

window{

  f:指向函数
// f:undefined   变量声明有冲突，会忽略
} 
```

处理函数冲突：

```javascript

<script type="text/javascript">
console.log(f);
var f=5;
function f(){
	console.log('111');
}
function f(){
	console.log('222');
}
</script>

```
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_4.png)

这充分的说明了JS引擎在处理函数同名冲突的时候，会覆盖上一个函数。同时也说明了JavaScript中函数是占据十分重要的位置。

## 1.3执行阶段
，
```javascript

<script type="text/javascript">
console.log(a);
console.log(b);  //会报错，应该注释掉，具体原因看图2所表达的意思
console.log(f);
console.log(g);

var a=5;      //var声明的局部变量
b=6;          //没有var声明的全局变量
console.log(b);
function f(){           //函数声明形式
	consolg.log('f');
}
var g=function(){    //函数表达式形式
	console.log('g');
}
console.log(g);
</script>

```

分析：

* 预理阶段:浏览器在运行过程中会扫描用声明的方式创建的函数，用var定义的变量

```javascript
  f:指向函数
  a:undefined
  g:undefined   //注意：g因为是函数表达式的形式，所以这里代表的是变量,值为undefined而不是函数
```

* 执行阶段:

```javascript
{
	f:指向函数
	a:5
    g:指向函数
	b:6
}
```
所以运行结果大家应该很清楚了吧，我把运行结果放着，大家可以参考下。
运行结果：

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_5.png)

总结：有var和没有var声明变量最主要的区别，

* 当有var声明变量时，在预处理阶段变量的值为undefined，在执行阶段变量的值为用户所赋给变量的值。

* 当没有var声明变量时，在预处理阶段变量的没有值，在执行阶段变量的值为用户所赋给变量的值。


# 2.函数预处理与执行


注意:全局预处理和函数预处理是人为分的，实际并不存在的，并不是说全局预处理阶段完成后才进入到函数处理阶段

## 预处理阶段

在全局预处理阶段创建的词法环境对象在浏览器中就是window对象，但是在函数预处理中阶段创建的词法环境对象是真实存在，看不见，摸不着的。也就是说，我们是访问不了的，因为这是JS解析器做的事情。

因为函数是有参数的，所有这个函数的参数已经就是预处理阶段词法环境对象的成员了。在预处理阶段的步骤如下：

* 每调用一次，产生一个LexicalEnvironment
* 先函数的参数
* 内部声明式函数
* 内部var变量
* 冲突情况与全局处理一样(函数覆盖，变量忽略)

```javascript

<script type="text/javascript">
function f(a,b){
	console.log(a,b);
	var b=100;
	function a(){

	}
}

f(1,2);
</script>
```

分析之前先看一个小案例：
```javascript
//如果声明一个函数
function A(a,b){

}
A(1);
//声明的时候有两个参数，但是调用的时候只有一个参数，那么它所产生的词法环境对象为
lexical env{
a:1;
b:undefined
	}
```
分析:
预处理阶段：
```javascript
lexical env{   //词法环境对象

    a:1
    b:2
    arguements:2  //实际穿的参数的个数
}

```
执行阶段
```javascript
lexical env{   //词法环境对象

    a:指向函数 //函数a有冲突，此时现有的函数会覆盖原有的函数
    b:2   //变量b有冲突，会忽略
    arguements:2  //实际穿的参数的个数
}

```
运行结果:

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_6.png)

## 执行阶段
执行阶段主要有两个作用如下：
* 给预处理阶段的成员赋值，参考上一个代码，给预处理阶段的数据成员和成员变量该赋值的赋值、该忽略的忽略、该覆盖的覆盖。视具体的情况而定



* 如果没有用var声明的变量，会成为最外部LexicalEnvironment(window,全局)
```javascript
<script type="text/javascript">
function f(){
	function g(){
		b=100;
	}
	g();
}
f();
</script>

```


我们可以发现运行这段代码并没有任何结果，那么大家肯定会有疑问b的值跑去哪里了，这时候没有用var声明的b变成了全局变量，跑到了最外部window对象的词法环境去了，而不是某一个函数的词法环境，所以可以通过`window.b`去访问它。

![ ](http://images.cnblogs.com/cnblogs_com/cliy-10/1232435/o_7.png)









