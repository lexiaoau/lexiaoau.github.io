---
author: lexiao
comments: true
date: 2019-05-14 01:35:24+00:00
layout: post
link: http://localhost/blog/?p=346
slug: c%e4%bb%a3%e7%a0%81%e4%be%8b%e5%ad%90
title: c++代码例子
wordpress_id: 346
categories:
- C++
---

### Constructor

1.1 使用格式

类为：

class Employee

{

Employee( char* );

};

Employee  e1 = Employee(“Mary”);

Employee  e1(“Mary”);

其它调用Constructor的例子：

Father f1(0);

Father *f2 = new Father();

Father f3 = Father(1);

Father f4 = 1;

1.2 规则

l 可以overload，只要参数列表不一样；

### 如何在头文件中声明一个不确定长度的数组？

做法是在 头文件中先声明并定义一个 常量值，

  


例如

  


static const int ARRAY_COUNT = 5;

  


然后在类里面声明数组：

  


private:

  


int oneArray[ARRAY_COUNT];
