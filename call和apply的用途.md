---
title: call和apply的用途
comments: true
date: 2018-07-16 19:00:20
categories: 博客列表
tags: JavaScript

---

## call和apply的用途

### 1.改变this指向

call和apply最常见的用途是改变函数内部的this指向

```
var obj1={
	name: "张三"
}

var obj2={
	name: "李四"
}
window.name="window";
function showname(){
	console.log(this.name);
}

showname()  //window(this指向window)
showname.call(obj1); //张三(this指向obj1)
showname.apply(obj2); //李四(this指向obj2)
```

当执行showname.call(obj1);时，showname函数体内的this就指向obj1对象，

```
function showname(){
	console.log(this.name);
}相当于
function showname(){
	console.log(obj1.name);
}
```

我们经常会遇到this指向被不经意改变的场景：

比如有一个div节点，div节点的onclick事件中的this本来是指向这个div的

```
document.getElementById("div").onclick=function(){
		console.log(this.id); //div
}
```

假如该事件函数中有一个内部函数func，在事件内部调用func函数时，func函数内部的this这时指向了window，而不是我们期望的div.

```
window.id="window";
document.getElementById("div").onclick=function(){
		console.log(this.id); //div
		function func(){
		console.log(this.id); //window
		console.log(this==window); //true
	}
  func();
}
```

我们可以用call来修正func函数内的this，使其指向this。

```
document.getElementById("div").onclick=function(){
		console.log(this.id); //div
		function func(){
		console.log(this.id); //div
		console.log(this==div); //true
	}
  func.call(this);
}
```

### 2.Function.prototype.bind()

bind()方法的主要作用就是将函数绑定至某个对象，bind方法()会创建一个函数，函数体内this对象的值会被绑定到传入bind()函数的值。

主流的浏览器都实现了内置的Function.prototype.bind()，用来指定函数内部的this指向。

我们可以自己模拟一个bind()方法。

```
Function.prototype.bind=function(context){
	var that=this;  //保存原函数
	return function(){ //返回一个新的函数
	 return that.apply(context,arguments);
	 //执行新的函数的时候，会把之前传入的context当做新函数体内的this。
  }
}
var obj={
	name:"张三"
}
var func=function(){
	console.log(this.name);//张三
}.bind(obj);
func();
```

通过Function.prototype.bind包装func函数，并且传入一个对象context当做参数，这个context对象就是我们想修正的this对象。

在Function.prototype.bind的内部实现中，我们先把func函数的引用保存下来，然后返回一个新的函数。当我们将来在执行func函数时，实际上先执行的是这个刚刚返回的新函数。在新函数内部，`that.apply(context,arguments)`这句代码才是执行原来的func函数，并且指定context对象为func函数体内的this。

我们可以封装一个更复杂的bind()方法，可以往func含糊是中预先填入一些参数。

```
Function.prototype.bind=function(){
	var that=this;  //保存原函数
	    context=[].shift.call(arguments); //取得外部传入的构造器(arguments=>obj)
	    args=[].slice.call(arguments);//剩下的参数转换成数组
	return function(){ //返回一个新的函数
	   return that.apply(context,[].concat.call(args,[].slice.call(arguments)));
	 //执行新的函数的时候，会把之前传入的context当做新函数体内的this。
	 //并且组合两次分别传入的context当做新函数体内的this
  }
};
var obj={
	name:"张三"
}
var func=function(a,b,c,d){
	console.log(this.name);//张三
	console.log([a,b,c,d]); //[1, 2, 3,4]
}.bind(obj,1,2);
func(3,4);
```

### 3.借用其他对象的方法

##### 3.1.借用构造函数

基于混合的对象冒充和原型方式实现继承

```
function A(name){
	this.name=name;
}
A.prototype.showname=function(){
	console.log(this.name);
}
function B(name){ //一定要带参数
	A.call(this,name);
}
B.prototype=new A();
var b=new B("李四");
console.log(b.showname());  //李四

console.log(b.__proto__==B.prototype) //true
console.log(B.prototype.__proto__==A.prototype) //true
```

通过借用构造函数，我们可以实现继承的效果。

函数的参数列表arguments是一个类数组，虽然它也有"下标"，但它并非真正的数组，所以不能像数组一样，进行排序操作或者往集合里添加一个新的元素,这时我们通常会借用Array.prototype对象上的方法。

举例：

往arguments中添加一个新的元素，通常会借用`Array.prototype.push.`

```
(function(){
	Array.prototype.push.call(arguments,3);
	console.log(arguments);  //[1,2,3]
})(1,2);
```

想要整理arguments中的元素，通常会借用`Array.prototype.sort`

```
(function(){
	Array.prototype.sort.call(arguments);
	console.log(arguments);  //[1,2,3]
})(5,2,6,1); //[1, 2, 5, 6]

```

##### 3.2.关于Array.prototype.push方法

Google-V8引擎源码中关于push方法的封装

```
function ArrayPush() {
  var n = TO_UINT32(this.length); // 被push的对象的length
  var m = %_ArgumentsLength(); // push 的参数个数
  for (var i=0; i<m; i++) {
     this[i+n] = %_Arguments(i); //复制元素(1)
   }
  this.length=n+m; // 修正length属性的值(2)
  return this.length;
};
```

通过上述代码可以看出Array.prototype.push方法实际上是一个属性复制的过程，把参数按照下标依次添加到被push的对象上面，顺便修改了这个对象的length属性。至于被修改的对象是谁，到底是数组还是类数组对象，这并不重要。

所以我们可以把"任意"对象传入Array.prototype.push;

```
var a={};
Array.prototype.push.call(a,"first","second");
console.log(a.length); //2
console.log(a[0]); //first
```

其实这里的`"任意"`对象是有条件限制的：

* 对象本身要可以存取属性

* 对象的length属性可读写

如果借用Array.prototype.push方法的不是一个object类型的数据，而是一个number类型的数据，会有什么效果？

```
var a=0;
Array.prototype.push.call(a,"first","second");
console.log(a.length); //undefined
console.log(a[0]); //firsundefined
```

我们无法在number类型上存取其他数据，所以一个number类型的数据不可能借用到Array.prototype.push方法

函数的length属性只是一个只读的属性，表示形参的个数，如果把一个函数传入Array.prototype.push会发生什么呢？

```
function func(){}
Array.prototype.push.call(func,"first","second");
console.log(func.length); //Cannot assign to read only property 'length' of function 'function func(){}'
```

