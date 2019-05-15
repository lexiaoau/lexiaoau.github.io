---
author: lexiao
comments: true
date: 2019-05-14 01:37:01+00:00
layout: post
link: http://localhost/blog/?p=425
slug: abstract-factory-%e6%a8%a1%e5%bc%8f
title: Abstract Factory 模式
wordpress_id: 425
categories:
- design pattern
---



  * 适用范围



    1. （情况一）product 如何被 created, composed, and represented 不应该影响 其系统；并且


    2. （情况二） 系统 中 有很多种 product系列，每次需要生产 其中一个系列。例如 Toyota（系统） 有 Camry（一个产品系列）、Corrola（一个产品系列），由客户指定是生产 Camry 还是 Corrola。并且


    3. （情况三） 要保证每个 产品系列 中的所有产品配套使用，不能和其它系列的搭配。例如，不能生产 Corrola 中的一款 和 Camry 中的两款。



[![](http://img.blog.163.com/photo/2wJ6JpTDZFI0pDNhxU5xcA==/1991998410181650290.jpg)](http://img.blog.163.com/photo/2wJ6JpTDZFI0pDNhxU5xcA==/1991998410181650290.jpg)






  * 模式思路



    1. 有一个 Abstract Factory ，里面 定义了 每个 abstract product 生产 的 interface。


    2. 每一个 产品系列 有一个 concrete factory，并且继承 abstract factory。这个 concrete factory 会实现每个 abstract product 生产 的 interface，实现如何生成 本系列的 product。


    3. Client 根据实际情况指定需要调用 那个 product 的生成。


  * 模式利弊



    1. Client 与 concrete factory 被 decouple了。 client 不需要知道如何生成 具体的 product，只需要通过 abstract interface 去生成。


    2. 易于更换 产品系列。一个 concrete factory 在程序中只会 出现一次——初始化时。所以，只需在这里改变初始化那个 factory，就可以改变实际使用的 产品系列。


    3. 保证了所有生成的产品都是属于同一个系列的，而不是mix。


    4. 坏处：加入新的产品比较困难。因为需要修改 abstract product 的 interface，而导致所有 concrete class 都需要修改。


  * 模式实现（一般做法）



    1. 把 factory 实现成 singleton 模式。 ——因为一个程序一般对每个产品系列只需要一个 factory object。


    2. concrete factory 要实现 Abstract Factory 中每个产品的生成，一般是每个product 有一个create函数。如果有很多个 产品系列，可以考虑使用 prototype 模式。


    3. 为了方便加入 新的产品，可以考虑在某些函数中用 参数 指定具体生成的是什么 object，但是这是和语言相关的，因为要 cast。
