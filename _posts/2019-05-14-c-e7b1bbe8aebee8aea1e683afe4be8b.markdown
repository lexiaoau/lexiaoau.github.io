---
author: lexiao
comments: true
date: 2019-05-14 01:36:53+00:00
layout: post
link: http://localhost/blog/?p=417
slug: c-%e7%b1%bb%e8%ae%be%e8%ae%a1%e6%83%af%e4%be%8b
title: C++ 类设计惯例
wordpress_id: 417
categories:
- C++
---



  1. 析构函数 ( destructor )



    1. 如果一个类需要作为基类，则 其析构函数 应该是 virtual 。 否则，通过 基类指针 删除 子类对象 时会出现不可预见后果。


    2. 如果一个类不需要被其它类继承，则 其析构函数 不应该是 virtual ，因为 virtual 函数需要额外消耗。
