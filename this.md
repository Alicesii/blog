---
title: this
comments: true
date: 2018-05-14 14:40:40
categories: 前端
tags: JavaScript

---

> JavaScript的this总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。

## 1.this的指向

关于this的指向可以分为四种情况

1.作为对象的方法调用

2.作为函数的实例化对象调用

2.作为普通函数调用

3.构造器调用

4.`Function.prototype.call`或`Function.prototype.apply`调用

### 1.1.作为对象的方法调用

**当函数作为对象的方法被调用时，this指向该对象。**

```
var obj={
	name:"张三",
	showname:function(){
	  console.log(this.name); //张三
	  console.log(this==obj) //true
    }
}
obj.showname(); //函数作为对象的方法被调用
```

### 1.2.作为对象的构造函数 被调用

**当函数作为对象的构造函数被调用时，this指向实例化的对象。**

```
function A(){
	this.name="张三";
	this.showname=function(){
	 console.log(this.name);   //张三
	 console.log(this==a);   //true
  }
}
var a=new A();
console.log(a.showname());//函数作为对象的构造函数被调用时
console.log(a.__proto__==A.prototype)  //true
```

### 1.3.作为普通函数调用

当函数不作为对象的属性被调用时，也就是普通函数方法，this指向全局对象，在浏览器的JavaScript里，这个全局对象是window对象。

```
window.name="张三";
function showname(){
	console.log(this.name); //张三
	console.log(this==window);//true
}
showname();//普通的函数调用
```

或者

```
window.name="张三";
var obj={
	name:"王五",
	showname:function(){
	  console.log(this.name);    //张三
      console.log(this==window);  //true
      }
 }
var showname=obj.showname;
console.log(showname());//普通的函数调用
```

举例：

在div节点的事件函数内部，有一个局部的callback方法，callback被作为普通函数调用时，callback内部的this这时指向了window，但我们往往想让它指向该div节点。

```
<body>
 <div id="div1">我是一个div</div>
</body>
<script>
	window.id="window";
	document.getElementById("div1").onclick=function(){
		console.log(this.id); //div1
		function callback(){
		    console.log(this==window);//true
			console.log(this.id) //window
		}
		callback();
	}
</script>
```

最简单的解决方案是，用一个变量保存div节点的引用。

```
<body>
 <div id="div1">我是一个div</div>
</body>
<script>
	window.id="window";
	document.getElementById("div1").onclick=function(){
		console.log(this.id); //div1
		var that=this;
		function callback(){
		    console.log(that==div1);//true
			console.log(that.id) //div1
		}
		callback();
	}
</script>
```

在ECMAScript5的strict模式下，这种情况下的this已经被规定为不会指向全局对象，而是undefined。

```
function func(){
	"user strict"
}
```

### 1.4.构造器调用

除了内置对象，大部分JavaScript函数都可以当作构造器使用，构造器函数的外表跟普通函数一模一样，它们的区别在于被调用的方式。当用new运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的this对象就指向返回的这个对象。

```
function showname(){
	this.name="张三";
	console.log(this); //showname {name: "张三"}

}
var getname=new showname();
console.log(getname.name);//张三
console.log(this==window); //true
```

如果构造器显示地返回一个Object类型的对象，那么this的指向是显示返回的那个对象，而不是我们之前期待的this。

```
function showname(){
	this.name="张三";
	return {
	 name:"李四"
     }
   console.log(this); //{name: "李四"}
}

var getname=new showname();
console.log(getname.name);//李四
console.log(this==window); //true
```

如果构造器不显式地返回任何数据，或者返回一个非对象类型的数据，this的指向就不是改变。

```
function showname(){
	this.name="张三";
	return name:"李四"
	console.log(this); //showname {name: "张三"}
}
var getname=new showname();
console.log(getname.name);//张三
console.log(this==window); //true
```

4.Function.prototype.call或Function.prototype.apply调用

利用`Function.prototype.call`或`Function.prototype.apply`可以动态的改变传入函数的this。

```
var obj1={
	name:"张三",
	showname:function(){
	console.log(this.name);
	console.log(this);
	//第一次调用：this的指向：{name: "张三", showname: ƒ}
	//第二次调用：this的指向：{name: "李四"}
  }
}
var obj2={
name:"李四";
}
console.log(this)//window
console.log(obj1.showname());//张三
console.log(this) //window
console.log(obj1.showname.call(obj2)); //李四
```

## 2.丢失的this

```
var obj={
	name:"张三",
	showname:function(){
	  console.log(this.name);
	  console.log(this==obj);
    }
}
obj.showname(); ////函数作为对象的方法被调用(张三和true)
var showname1=obj.showname;
console.log(showname1()); //普通的函数调用(undefined和false)this->window
```

当调用obj.showname()时，showname()方法是作为obj对象的属性被调用的，此时的this指向obj对象。

当用变量showname1来引用obj.showname，并且调用showname1()时，此时是普通函数调用方式，this是指向全局window的。

## 3.关于document.getElementById

用document.getElementById获取元素ID值时，这个方法名实在是太长了，我们可以封装一下。

```
window.onload=function(){
function getId(id){
	return document.getElementById(id);
}
   var div=getId("div"); //获取到元素的ID值
   console.log(div.id); //div
}
```

我们是不是还可以用更加简便的方式：

```
var getId=document.getElementById;
var div=getId("div"); //获取到元素的ID值
console.log(div.id); //报错

```

可以尝试在浏览器中运行一下：

```
<body>
 <div id="div">我是一个div</div>
</body>
<script>
window.onload=function(){
var getId=document.getElementById;
getId("div");
}
</script>
```

答案：不可以

浏览器引擎的document.getElementById方法的内部实现中需要用到this，this本来被期望指向document，当getElementById方法当做document对象的属性被调用时，方法内部的this确实指向document，但当用getId来引用document.getElementById之后，再调用getId，此时就成了普通函数调用，函数内部的this指向了window，而不是document。

我们可以使用apply或者call把document当做this传入getId函数，帮助`"修正"`this.

```
document.getElementById=(function(func){
	return function(){
	   return func.call(document,arguments);
   }
})(document.getElementById)

var getId=document.getElementById;
var div=getId("div");
console.log(div.id); //div
```

