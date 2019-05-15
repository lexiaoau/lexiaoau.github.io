---
author: lexiao
comments: true
date: 2019-05-14 01:35:15+00:00
layout: post
link: http://localhost/blog/?p=343
slug: java-inner-class%e5%86%85%e9%83%a8%e7%b1%bb
title: java inner class内部类
wordpress_id: 343
categories:
- java
---

内部类：定义在其他类里面的类。  
使用内部类的理由：  
1.内部类方法能够访问外部类的任何数据成员包括私有成员。  
2.对同一个包的其他类，内部类是不可见的。  
3.匿名内部类能够方便的定义回调而不用写太多代码。

  


  


  



				

非静态内部类没有默认的构造函数(无参数的)，非静态内部类的构造函数都有一个final的外围类对象引用this$0。  
内部类的特殊语法规则：  
1.相对内部类，引用其外部类隐式对象的形式：OuterClass.this  
2.调用内部类的构造函数：outerObject.new InnerClass(construction parameters);  
3.外部类外面引用内部类：OuterClass.InnerClass
