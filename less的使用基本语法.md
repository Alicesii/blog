---
title: less的基本使用
comments: true
date: 2018-08-09 17:23:16
categories: 博客列表
tags: Less
about:

---

## 1.demo.less

```
/*LESS的形式*/
.content {
    ul {
        list-style: none;
    }
    li {
        height: 25px;
        line-height: 25px;
        padding-left: 35px;
        background: url(../image/hot.gif) no-repeat center left;
        a {
            text-decoration: none;
            color: #535353;
            font-family: "microsoft yahei";
        }
    }
}
```

## 2.variables.less

### 2.1.普通的变量

```
@green:green;
@white:white;
header{
    background-color: @green;
    h1{
       color: @white;
    }
}
footer{
    background-color: @green;
    color: @white;
}
```

### 2.2.作为选择器和属性名

```
@width:width;

.@{width}{
    @{width}:150px;
    height: 60px;
}
```

### 2.3.作为URL

```
@Imgurl:"http://seopic.699pic.com/";
body{
    background-image: url("@{Imgurl}photo/50056/3725.jpg_wh1200.jpg");
    background-repeat:no-repeat;
    width:200px;
}
```

### 2.4.延迟加载

* 变量是延迟加载的，在使用前不一定要预先声明，可以先使用后定义

* 定义多个相同名称的变量时，只会使用最后定义的变量，Less会从当前作用域中向上搜索，这个行为类似于CSS的定义中始终使用最后定义的属性值。

```
@var:0;
.class{
    @var:1;
    .brass{
        @var:2;
        three:@var;  //three的值为3
        @var:3;
    }
    one:@var; //跳出.bress,one的值为1
//  two:@var;
//  @var:aaa;
//注意：只能在它的作用域范围找，
}
```

## 3.mixins1.less

### 3.1.普通混合

混合就是一种将一系列属性从一个规则集引入`('混合')`到另一个规则集的方式

```
.font_hn {
    color: red;
    font-family: "仿宋";
}

.p1 {
    font-size: 28px;
    .font_hn;
}

.p2 {
    font-size: 24px;
    .font_hn;
}
```

### 3.2.不带输出的混合

* 如果你想要创建一个混合集，但是却不想让它输出到你的样式中

* 在不想输出的混合集的名字后面加上一个括号，就可以实现

```
.font_hn() {
    color: red;
    font-family: "仿宋";
}

.p1 {
    font-size: 28px;
    .font_hn;
}

.p2 {
    font-size: 24px;
    .font_hn;
}
```

### 3.3.带选择器的混合

```
.my-hover-mixin() {
    &:hover {
        //&：代表父级
        border: 1px solid red;
    }
    //&:hover=.my-hover-mixin:hover
}

button {
    .my-hover-mixin();
}

a {
    .my-hover-mixin();
}
```

### 3.4.带参数的混合

```
.border1(@color) {
    border: 1px solid @color;
}

h1 {
    &:hover {
        .border1(green);   //一定要有参数，不然会报错
    }
}
```

### 3.5.带参数并且有默认值

```
.border2(@color:red) {
    border: 1px solid @color;
}
h6 {
    &:hover {
        .border2();
    }
}
```

### 3.6.当有默认参数的时候，如果.`.border()`有参数，并不会报错，会执行默认的参数

```
h6 {
    &:hover {
        .border2(pink);
    }
}
```

### 3.7.`.border()`有参数时，会执行参数里面的值

## 4.mixins2.less

### 4.1.带多个参数的混合

* 参数以分号或逗号分隔，建议使用分号。

* 符号逗号具有双重含义：它可以解释为混合参数分隔符或CSS列表分隔符。

```
.mixin1(@color;@padding:xxx;@margin:2){
    color-3:@color;
    padding-3:@padding ;
    margin: @margin@margin@margin@margin;
}
.divmaizi1{
    .mixin1(red;)
}
.maizi1{
    .mixin1(1,2,3;something,else;132); //一个分号结束为一个参数，【以分号为分隔符】
}
.div1{
    .mixin1(1,2,3);  //若没有分号，只有逗号，括号里面的所有参数则分别代表不同的值，【以逗号为分隔符】
}
.div2{
    .mixin1(1,2,3;);  //若既有分号又有逗号，分号前的所有参数只分别代表1个整体参数，【以分号为分隔符】
}
```

### 4.2.定义多个具有相同名称和参数数量的混合

* 定义多个具有相同名称和参数数量的mixin是合法的，Less会使用它可以应用的属性。

* 如果使用mixin的时候只带一个参数，这个属性会导致所有的mixin。

```
.mixin2(@color){
    color-1:@color ;
}
.mixin2(@color;@padding:2){
    color-2: @color;
    padding-2:@padding ;
}
.mixin2(@color;@padding;@margin:2){
    color-3:@color ;
    padding-3:@padding ;
    margin:@margin@margin@margin@margin;
}
.some .selector div{
    .mixin2(#008000);
}
```

### 4.3.命名参数

* mixin参考可以通过名称而不是位置来提供参数值。任何参数都可以通过其名称来引用，而且它们不必按照任何特殊顺序进行引用

```
.mixin3(@color:black;@margin:10px;@padding:20px){
    color: @color;
    margin: @margin;
    padding: @padding;
}
.class1{
    .mixin3(@margin:20px;@color:#33acfe);
}
.class2{
    .mixin3(#efca44;@padding:40px;);
}
.class3{
    .mixin3(@padding:20px;)
}
```

### 4.4.arguments变量

* `@arguments`代表所有可变的参数，参数的项先后顺序就是你的`()`括号内的参数的先后顺序。

* 在使用赋值时：值的位置和个数也是一一对应的。

若只有一个值：把值赋值给第一个；

若有两个值：把值赋给第一个参数和第二个参数......以此类推。

```
.border1(@x:solid,@c:red){
    border:21px @arguments;
}
.div3{
    .border1(solid);
}
```

### 4.5.匹配模式：传值的时候定义一个字符，在使用的时候使用哪个字符，就调用哪条规则。

```
.border2(all,@w:10px){
    border-radius: @w;
}
.border2(t_l,@w:10px){
    border-top-left-radius: @w;
}
.border2(t_r,@w:10px){
    border-top-right-radius: @w;
}
.border2(b_l,@w:10px){
    border-bottom-left-radius: @w;
}
.border2(b_r,@w:10px){
    border-bottom-right-radius: @w;
}
```

### 4.6.可以传参数，也可以不传参数

```
button{
    .border2(all);
}
button{
    .border2(all,5px);
}
button{
    .border2(t_l,5px);
}
button{
    .border2(t_r,5px);
}
button{
    .border2(b_l,5px);
}
button{
    .border2(b_r,5px);
}
```

### 4.7.得到混合中变量的返回值

```
.average(@x,@y){
    @average:((@x+@y)/2);
    @add:(@x+@y);
}
div{
    .average(16px,50px);
    padding: @average;
    margin: @add;
}
```

## 5.nested-rules.less

### 5.1.嵌套规则

嵌套规则模仿了HTML的结构，让我们的css代码更加简洁明了，清晰。

* 传统的写法

```
//header{
//  width:960px;
//  background-color: cornsilk;
//}
//header p{
//  font-size: 26px;
//  color:#535353;
//  font-family: "仿宋";
//}
//header .logo{
//  width: 200px;
//  height: 200px;
//  border:2px solid #008000;
//}
```

* LESS写法

```
header {
    width: 960px;
    background-color: cornsilk;
    p {
        font-size: 26px;
        color: #535353;
        font-family: "仿宋";
    }
    .logo {
        width: 200px;
        height: 200px;
        border: 2px solid #008000;
    }
}
```

### 5.2.父元素选择器：`&`表示当前选择器的所有父选择器

* 传统的写法

```
//header .logo{
//  width: 200px;
//  height: 200px;
//  border:2px solid #008000;
//}
//header .logo:hover{
//  background-color: #EFCA44;
//
//}
```

* LESS写法

```
header {
    width: 960px;
    background-color: cornsilk;
    p {
        font-size: 26px;
        color: #535353;
        font-family: "仿宋";
    }
    .logo {
        width: 200px;
        height: 200px;
        border: 2px solid #008000;
        &:hover{   //&符号代表它的所有的父元素选择器
            background-color: coral;
        }
    }
}
```

### 5.3.改变选择器的顺序

将`&`放到当前选择器之后，就会把当前选择器插入到所有的父选择器之前

```
.a{
    .b{
        &.c{       //选择当前的选择器 ,选择器顺序为.a .b .c
            color:bisque;
        }
    }
}
.a{
    .b{
        .c &{      //C的选择器将会被提到所有的父选择器之前.c .a .b
            color:bisque;
        }
    }
}
```

### 5.4.组合使用生成所有可能的选择器列表

* `[& &]`(表示两个组合)，要有一个空格

* `[& & &]`(表示三个组合)

* `[& & & &]`(表示四个组合)

* ......以此类推。

```
p,a,ul,li{
    border-top: 2px dotted #366;
    &{                 //当有一个&时，表示选择当前的选择器
        border-top: 0;
    }
}
p,a,ul,li{
    border-top: 2px dotted #366;
    & &{              //当有两个&&时，表示混合所有的选择器
        border-top: 0;
    }
}
a,b,c{
    & &{
        color:aqua;
    }
}
```

## 6.operation.less

### 6.1.运算说明

* 任何数值、颜色和变量都可以进行运算

* LESS会为你自动推断数值的单位，所以你不必为每一个值都加上运算

* 运算符与值之间必须以空格分开，涉及优先级时以0进行优先级运算

```
//.wp{
//  margin: 0 auto;
//  background-color: #FFC0CB;
//  width: 450px+450;
//  height: 400+400px;
//}
```

### 6.2.数值型运算：优先级运算[使用括号区分优先级]

```
//.wp{
//  margin: 0 auto;
//  background-color: #FFC0CB;
//  width:(550-50)*2px ;
//  height: 400+400px;
//}
```

### 6.3.颜色值运算：先将颜色值转换为rgb模式,然后再转换为16进制的颜色值并且返回

```
.wp{
    margin: 0 auto;
    width:(550-50)*2px ;
    height: 400+400px;
    background-color:#000000+21;
    //rgb(0,0,0)+21 =rgb(21,21,21) =#151515
    //rgb模式的取值范围是(0~255，0~255，0~255)，
   //当你加的值超过了255之后，还是会按照最大值255来计算
   //不能直接使用颜色的英文名称来进行运算(例如用red+21)
}
```

## 7.Function.less

LESS中提供了转换颜色、处理字符串和进行算术运算的函数。

### 7.1.rgb()函数：将rgb函数的值转换为16进制的值

```
.bgcolor{
    background-color:rgb(255,0,0); //#ff0000
}
```

### 7.2.提取颜色值的函数`[blue]|[red]|[green]`，提取的是rgb模式的颜色值

```
.bgcolor1{
    z-index: blue(#5c7abd); //提取蓝色的值
    border: red(#565656);//提取红色的值
}
```

## 8.命名空间

```
#bgcolor(){
    background-color: #FFFFFF;
    .a{
        color:#888888;
        &:hover{
            color:#ff6600;
        }
        .b{
            background-color: #ff0000;
        }
    }
}
.wi{
    background-color: #008000;
    color:#fff;
    .a{
        color: #008000;
        background-color: #FFFFFF;
    }
}
.bgcolor1{
    background-color: #fdfeeo;
    #bgcolor>.a;
}
.bgcolor2{
    .wi>.a;
}
```

## 9.作用域

LESS中的作用域与编程语言中的作用域概念非常相似，首先会在局部查找变量和混合，如果没有找到，编译器就会在父作用域中查找，以此类推。

```
@color:#fff;
.bgcolor{
    width:50px;
    a{
      color: @color;
    }
    @color:#ff0000;
}
@color:#fff;
.bgcolor1{
    width:50px;
    a{
        @color:green;
      color: @color;
    }
    @color:#ff0000;
}
```

## 10.import.less

### 10.1.可以引入一个或者多个less文件，然后这个文件中的所有变量都可以在当前的less项目中使用

```
@import "main.less";
.content{
    width: @wp;
    height: @wp;
}
```

### 10.2.引入CSS文件会被原样输出到编译的文件中

`@import "index.css";`

### 10.3.可带参数:

* once[默认、使用一次]

* reference[使用LESS文件但不输出]

* inline[在输出中包含源文件但不加工它]

* less[将作为LESS文件对象，无论是什么文件扩展名]

* css[将作为CSS文件对象，无论是什么文件扩展名]

* multiple[mutiple]

## 11.important.less

在调用的混合集后面追加`!important`关键字，可以使混合集里面的所有属性都继承`!important`。

```
.foo(@bg:#515151,@color:#900){
    background-color: @bg;
    color: @color;
    font-size: 16px;
    font-weight: 900;
}
.unimport{
    .foo();
}
.important{
    .foo()!important;
}
```

## 12.条件表达式

### 12.1.带条件的混合

```
.mixin(@a)when(lightness(@a)>=50%){  //lightness:取颜色值亮度的函数,取值为0~100%
   background-color: black;
}
.mixin(@a)when(lightness(@a)<50%){ //rgb模式中最大的取值为255，除以2为175.5,我们可以粗略的认为:
    background-color: white;         //当a的值大于175.5时为亮色，a的值小于175.5时为暗色。
}
.mixin(@a){
    color:@a;
}
.class1{
    .mixin(#ddd);//211>175.5>50%,显示的背景颜色为黑色
}
.class2{
    .mixin(#555); //85<175.5<50%,显示的背景颜色为白色
}
```

### 12.2.类型检查函数：可以基于值的类型来匹配函数

* `[iscolor]`:

* `[isnumber]`:

* `[isstring]`:

* `[iskeyword]`:

* `[isurl]`:

### 12.3.单位检查函数:检查一个值除了数字是否是一个特定的单位

* `[ispixel]`：px

* `[ispercentage]`:百分比

* `[isem]`:厘米

* `[isunit]`:单元

## 13.loop循环

```
//.loop(@counter)when(@counter>0){
//  .loop((@counter)-1); //递归调用自身4 3 2 1 0
//  width: (10px*@counter);//每次调用时产生的样式代码 50px 40px 30px 20px 10px
//}
//
//div{
//  .loop(5); //调用循环
//}

//.loop(@counter)when(@counter>0){
//  h@{counter}{
//      padding: (10px*@counter);
//  }//每次调用时产生的代码样式
//  .loop((@counter)-1); //递归调用自身
//}
//div1{
//  .loop(6); //调用循环
//}

.loop(@counter)when(@counter<=6){
    h@{counter}{
        padding: (10px*@counter);
    }//每次调用时产生的代码样式
    .loop((@counter)+1); //递归调用自身
}
div1{
    .loop(1); //调用循环
}
```

## 14.合并属性

### 14.1.在需要合并的属性的:的前面加上`"+"`就可以完成合并，合并时以`","`逗号分割属性。

```
//.mixin(){
//  box-shadow+: inset 0 0 10px #555;
//}
//.myclass{
//  .mixin();
//  box-shadow+: 0 0 20px black;
//}
```

### 14.2.在需要合并的属性的:的前面加上"+`_"`就可以完成合并，合并时以`" "`空格分割属性。

```
.a(){
    background-color+_: #f60;
   background-image+_:url(../image/black.png);
}
.myclass{
    .a();
}
```

## 15.function

### 15.1.其他函数

### 15.2.字符串函数

### 15.3.长度相关函数

### 15.4.数字函数

### 15.5.类型函数

### 15.6.颜色值定义函数

### 15.7.颜色值通道提取函数

```
body{
//background:color();
background:url(../image/yinghua.jpg);
default();
unite();
e();
escape();
}
```

## main.less

* `@wp:960px;`
