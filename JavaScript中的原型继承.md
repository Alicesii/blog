---
title: JavaScript中的原型继承
comments: true
date: 2018-05-12 11:20:50
categories: 前端
tags: JavaScript

---

## 原型继承

在JavaScript中有几条原型继承的原则：

1.所有的数据都是对象

2.要得到一个对象，不是通过实例化，而是找到一个对象作为原型并克隆它。

3.对象会记住它的原型

4.如果对象无法响应某个请求，它会把这个请求委托给它自己的原型。

### 1.所有的数据都是对象

JavaScript的数据类型分为原始数据类型和引用数据类型，

原始类型的值包括5种，分别是undefined、Null、Bollean、Number、和String。

原始数据类型中除了null、undefined剩下的三个Bollean、Number、和String都是伪对象。

引用数据类型中所有的其他类都是由Object类继承而来，所以所有的引用数据类型都是对象。

```
Array.prototype.__proto__==Object.prototype  //true

Date.prototype.__proto__==Object.prototype  //true

Function.prototype.__proto__==Object.prototype  //true

String.prototype.__proto__==Object.prototype //true

RegExp.prototype.__proto__==Object.prototype  //true

Number.prototype.__proto__==Object.prototype  //true
```

所以说所有的数据都是对象，一点也不为过吧。

JavaScript中的根对象是Object.prototype对象。Object.prototype对象是一个空的对象，JavaScript中的每个对象，实际上都是从Object.prototypr对象克隆来的，Object.prototype对象就是它们的原型。

```
var obj1=new Object();

var obj2={};

console.log(Object.getPrototypeOf(obj1)==Object.prototype);  //true

console.log(Object.getPrototypeOf(obj1)==Object.prototype);  //true

console.log(obj1 instanceof Object);  //true
console.log(obj1 instanceof Object);   //true

```

### 2.要得到一个对象，不是通过实例化，而是找到一个对象作为原型并克隆它。

在JavaScript语言中，我们并不需要关心克隆的细节，因为这是引擎内部负责实现的，我们所需要的做的只是显示地调用`var obj1=new Object();或者var obj2={};`。此时引擎内部会从`Object.prototype`上面克隆出一个对象出来，我们最终得到的就是这个克隆出来的对象。

构造函数+原型方式创建对象：

```
function Person(name){
	this.name=name;
}
Person.prototype.showname=function(){
	console.log(this.name);
}
var a=new Person("张三");
console.log(a.name);   //张三
console.log(a.getname());  //张三
console.log(Object.getPrototypeOf(a)==Person.prototype);  //true
console.log(a instanceof Person);  //true
a.__proto__==Person.prototype;
Person.prototype.__proto__==Object.prototype;
```

注意：Person表示的不是类，而是函数构造器，JavaScript的函数既可以作为普通函数被调用，也可以作为构造器被调用。当使用new运算符来调用函数时，此时的函数就是一个构造器。

用new运算符创建对象的过程，实际上也是先克隆`Object.prototype`对象，再进行一些其他额外操作的过程。

new运算的过程：

```
function Person(name){
	this.name=name;
}
Person.prototype.showname=function(){
	console.log(this.name);
};

function newPerson(){
   var obj=new Object();  //从Object.prototype上克隆一个空的对象
   Constructor=[].shift.call(arguments); //取得外部传入的构造器(arguments=>Person)
   obj.__proto__=Constructor.prototype;  //指向正确的原型 obj.__proto__==Person.prototype
   var ret=Constructor.call(obj,arguments);   //借用外部传入的构造器给obj设置属性(arguments=>"张三")
   return typeof ret === 'object' ? ret : obj;  //确保构造器总是会返回一个对象
};

var a=newPerson(Person,"张三");
console.log(a.name);   //张三
console.log(a.showname());  //张三
console.log(Object.getPrototypeOf(a)==Person.prototype);  //true
console.log(a instanceof Person);   //true
```

### 3.对象会记住它的原型

对JavaScript的真正实现来说，并不能说对象有原型，只能说对象的构造器有原型。与其说"对象把请求委托给它自己的原型"，还不如说"对象把请求委托给它的构造器的原型"。

`__proto__`属性:某个对象的`__proto__`属性默认会指向它的构造器的原型对象`{Constructor}.prototype`。

var obj=new Object();
console.log(obj.__proto__==Object.prototype)  //true

`__proto__`就是对象与"对象构造器的原型"联系起来的纽带，对象通过`__proto__`属性来记住它的构造器的原型。


### 4.如果对象无法响应某个请求，它会把这个请求委托给它自己的原型。

当一个对象无法响应某个请求的时候，它会顺着原型链把请求传递下去，直到遇到一个可以处理该请求的对象为止。

虽然JavaScript的每个对象都是由Object.prototype对象克隆而来的，但对象构造器的原型并不仅限于Object.prototype,而是可以动态指向其他对象。也就是说，当对象a需要借用对象b的能力时，可以有选择性地把对象a的构造器的原型指向b，从而达到继承的效果。

```
function A(name){
this.name=name;
}

A.prototype.showname=function(){
	console.log(this.name);
}

var a=new A("张三");

console.log(a.name); //张三

console.log(a.__proto__==A.prototype)  //true
```

在执行上述代码时，搜索引擎做了哪些事？

第一步：尝试遍历对象a中的所有属性，但没有找到name这个属性。

第二步：查找name属性的这个请求被委托给对象a的构造器的原型(A.prototype)。它被`a.__proto__`记录着并且指向A.prototype。

第三步：在函数A中找到了name属性，并返回它的值。


通过原型的方式实现继承。

```
function A(){
}
A.prototype.name="张三";

function B(){
}
B.prototype=new A();

var b=new B();
console.log(b.name);  //张三
console.log(b.age);  //undefined
console.log(b.__proto__==B.prototype); //true
console.log(B.__proto__==A.prototype); //false
console.log(B.prototype.__proto__==A.prototype)//true
console.log(B.__proto__==A.__proto__)//true;
console.log(A.__proto__==Object.__proto__); //true
console.log(A.prototype.__proto__==Object.prototype)//true
```

在执行上述代码时，搜索引擎又做了哪些事？

第一步：尝试遍历对象a中的所有属性，但没有找到name这个属性。

第二步：查找name属性的请求被委托给对象B的构造器的原型(B.prototype)。它被`b.__proto__`记录着并且指向B.prototype。而B.prototype被设置为一个通过new A()创建出来的对象。

第三步：在该对象中依然没有找到name属性，于是请求被继续委托给这个对象构造器的原型A.prototype。

第四步：在A.prototype中找到了name属性，并返回给它的值。

原型链并不是无限长的，假设我们想要访问对象b的age属性，而对象b和它的构造器的原型上都没用age属性，那么这个请求会被最终传递到哪里去呢？

当请求到达A.prototype，并在A.prototype中也没有找到age属性的时候，请求会被传递给A.prototype的构造器原型Object.prototype，显然Objec.prototype中也没有age属性，并且Objec.prototype的原型是null，所以这次请求就到头了，b.age返回undefined。


