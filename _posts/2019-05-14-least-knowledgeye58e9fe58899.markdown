---
author: lexiao
comments: true
date: 2019-05-14 01:36:25+00:00
layout: post
link: http://localhost/blog/?p=389
slug: least-knowledgey%e5%8e%9f%e5%88%99
title: Least Knowledgey原则
wordpress_id: 389
categories:
- design rules
---



  * 原则核心：只和“最接近的朋友”交谈


  * 原则要求：对某个object的任何函数实现中， 只能调用以下函数：



    * 该object自身的函数；


    * 作为函数参数传递进来的object；


    * 由该函数创建、实例化的object；


    * 该object的 任何components


  * 原则要求不可以：（不可以）调用属于“由其它函数调用返回的object”的函数（not to call methods on objects that were returned from calling other methods)
