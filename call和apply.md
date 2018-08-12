---
title: call和apply
comments: true
date: 2018-07-15 13:20:20
categories: 博客列表
tags: JavaScript

---

> ECMAScript3给Function的原型定义了两个方法`Function.prototype.call`或`Function.prototype.apply`，在一些函数式风格的代码编写中，`call()`方法和apply方法扮演着及其重要的角色，它们所实现的功能是一模一样的，区别仅仅在于传入参数形式的不同。

## 1.apply方法

apply()方法接受两个参数，第一个参数指定了函数体内this对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply方法把这个集合中的元素作为参数传递给被调用的函数。

```
function func(a,b,c){
  console.log([a,b,c]); //[1, 2, 3]
}
func.apply(null,[1,2,3])
```
在上述代码中，参数1、2、3被放在数组中一起传入func函数，它们分别对应func参数列表中的a、b、c。

## 2.call方法

call()方法传入的参数数量不固定，第一个参数同样指定了函数体内this对象的指向，从第二个参数开始往后，每个参数被依次传入函数。

```
function func(a,b,c){
  console.log([a,b,c]); //[1, 2, 3]
}
func.call(null,1,2,3)
```
当调用一个函数时，JavaScript的解释器并不会计较形参和实参在数量、类型以及顺序上的区别，JavaScript的参数在内部就是用一个数组来表示的，从这个层面来说，apply比call的使用率更高。

如果我们想要明确地知道函数接受多少参数，而且想一目了然地表达形参和实参的对应关系，可以使用call来传递参数。

使用call或者apply时，如果传入的第一个参数是null，函数体内的this会指向window。

```
function func(a,b,c){
  console.log([a,b,c]); //[1, 2, 3]
  console.log(this==window ); //true
}
func.call(null,1,2,3)
```
如果是在严格模式下，函数体内的this为null

```
function func(a,b,c){
  "use strict"
  console.log([a,b,c]); //[1, 2, 3]
  console.log(this==undefined); //true
}
func.call(null,1,2,3)
```
有时使用call和apply的目的并不在于指定this指向，而是可以借用其他对象的方法，我们可以传入null来代替某个具体的对象。

```
Math.max.apply(null,[3,5,6,8,1]); //8

Math.min.apply(null,[3,5,6,8,1]);  //1

Math.random.apply(null,[0,1])  //0.6309077036021082
```
Math对象是内置对象，不用实例化就可以直接使用。