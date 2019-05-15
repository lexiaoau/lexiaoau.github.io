---
author: lexiao
comments: true
date: 2019-05-14 01:36:55+00:00
layout: post
link: http://localhost/blog/?p=419
slug: state-pattern
title: State pattern
wordpress_id: 419
categories:
- design pattern
---



  * 适用范围



    1. 当一个对象的适当行为是由其状态（state）决定时，而且该对象有很多个state时。


    2. [](http://img.blog.163.com/photo/2wJ6JpTDZFI0pDNhxU5xcA==/1991998410181650290.jpg)


  * 模式思路



    1. 有一个 context 类，该类作为 client 与 state object 间的 双向 桥梁， 可以 从 client 向 state 传递 configuration，也可以把自身传递给 state，让 concrete state 在处理时可以访问 context（如果有必要的话）。


    2. state 是一个 类 hierachy，一个抽象父类，其每一个子类代表一个具体状态。所有类都实现共同的接口。


    3. Context 负责控制 state 的变化（即现在是进入那个 state class），然后把相应的request 交给对应的 state 类去处理 。


  * 模式利弊



    1. 使得 state 的种类分明，state 转换也非常明显。


    2. 添加新的 state 相对容易，只需添加一个新的 state class。


    3. 把每个state 的 behaviour 封装在类的内部，修改也是 透明的。


    4. 一个不好的地方是，使得类 比较多而且分散。


  * 模式实现



    1. state 转换 的 几种实现方法。



      1. 由 context 类来实现 状态转换。


      2. 更为弹性和适当的方法是， 在 每个concrete state 里面，在相关处理中进入下一个state。


      3. 还有一种做法是 用一个表，来mapping 输入 和 状态。不过，表的查找，效率很低。


    2. state object 构造和删除 的 几种实现方法。



      1. 在需要时才建立，使用完后就删除。



        1. 适合不是经常访问 state 的情况下。


        2. 适合 state object 保存大量信息的时候。


      2. 提早建立，不删除。



        1. 适合经常访问 state 的情况。
