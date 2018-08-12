---
title: 三种方式制作ICON
comments: true
date: 2018-06-25 15:12:11
categories: 博客列表
tags: ICON
img:

---

**1.传统的引入背景图片的格式：**

`background：url("")`

**2.使用base64格式图片制作ICON**

Base64就是一种基于64个可打印字符来表示二进制数据的方法，如果使用base64格式，就不会去写url地址，就只是填一個字符串。

`url(data:image/png;base64,{img_data})`

* 优点：Base64图片可以减少请求，利于页面直接加载，加快首屏数据的显示速度

* 缺点：字符串未经压缩，体积比较大，维护不方便


**使用CSS3制作ICON**

常用属性：

```
border-radius：设置圆角
border-shadow：设置阴影
transform：设置图形的转换
```

* 优点: 体积较小，因为用几行代码就可以实现ICON的效果,特别适用与web APP,完全不用考虑兼容性的问题

* 缺点：不易于维护，存在兼容性的问题，对于传统浏览器支持性能不太好。

* 注意：CSS3 ICON用在一些规则的图形上，图形不会很复杂。
