---
author: lexiao
comments: true
date: 2019-05-14 01:37:00+00:00
layout: post
link: http://localhost/blog/?p=424
slug: factory-%e6%a8%a1%e5%bc%8f
title: Factory 模式
wordpress_id: 424
categories:
- design pattern
---



  * 适用范围



    1. 根本目的： 把 _在父类design时_ _不能决定的具体create类型_ 推迟到 子类中 去决定。


    2. （情况一）一个类 无法预见 所需创建的 object 的 class。 或者


    3. （情况二）一个类 希望 其子类 来决定生成的 object 的 类型


    4. （情况三）一个类 把其功能 移交给 几个“工具类”来实现，而 designer 希望自己指定由那一个“工具类”来实现。



[](http://img.blog.163.com/photo/2wJ6JpTDZFI0pDNhxU5xcA==/1991998410181650290.jpg)[![Factory 模式 - xiao le - me](http://img.blog.163.com/photo/ZYFRpI1MkSmXVR-s9TLOIg==/4791548528545668661.jpg)](http://img.blog.163.com/photo/ZYFRpI1MkSmXVR-s9TLOIg==/4791548528545668661.jpg)









  * 模式思路



    1. 有一个 application 类，其中的某个函数会 create 一个 （abstract） document 对象 ，该函数 被定义为 可重写的（override）。（此时有那些 concrete document 类并不清楚）


    2. sub-application 类 重写 父类中的函数，并实现 生成 它所需要的 某一个 concrete 的 document 对象。


  * 模式利弊



    1. 通过调用 父类 的 可重写的create函数，使得代码可以更 flexible 地处理 任何自定义的 新类。


    2. 一个不好的地方是，为了 重写create函数，必须 继承 application父类，即使这种继承不是希望的。


    3. 一个特点是可以为 平行对应 的 class hierachy 搭建桥梁。例如，先有一个 class hierachy，然后需要有 第二个 根据第一个 而生成的 hierachy。第一个 hierachy中的 类 可以通过改写其 factory 函数 来指定 它与 第二个 hierachy 中的那个类绑定。


  * 模式实现（一般做法）



    1. Creator类 的 几种实现方法。



      1. 把父类 声明成为 abstract 的类 并且 不为 factory 函数提供 默认实现。


      2. 把父类 声明成为 concrete 的类 并且 为 factory 函数提供 默认实现


    2. Factory 函数 的 几种实现方法。



      1. 不带 _与所生成的类相关的参数_。这种情况下，每个 子类中 的 factory 函数只能 生成一种 object。


      2. 带 _与所生成的类相关的参数_。这种情况下，每个 子类中 的 factory 函数可以生成多种 object，具体类型由参数指定。实现时，还可以指定如果参数不可识别时应该生成那种 object，类似 {switch...case..} 中的 default 语句。


    3. 在 C++ 中，可以用 template 考虑避免 必须继承 的坏处。


    4. 使用命名规则，帮助标志是否使用了 factory pattern。


  * 与 abstract factory 模式比较



    1. 使用目的不同。



      1. abstract factory 使用于 有多个 product family 的情况。


      2. factory 使用于 在父类定义时 子类类型仍然是未知 的情况。


    2. 模式思路是类似的，都是在 父类 中先声明一个 返回通用类型的 接口。然后 由子类重写该接口以 指定实际返回的类。client 都是调用 父类通用接口 而无需 担心 返回的具体类型。


    3. 工厂方法采用的是类继承机制（生成一个子类，重写该工厂方法，在该方法中生产一个对象）。而抽象工厂采用的是对象组合机制，专门定义“工厂”对象来负责对象的创建。对象组合的方式就是把“工厂”对象作为参数传递。  

    4. 工厂方法模式：
    <blockquote style="margin: 0 0 0 40px; border: none; padding: 0px;">一个抽象产品类，可以派生出多个具体产品类。   </blockquote><blockquote style="margin: 0 0 0 40px; border: none; padding: 0px;">一个抽象工厂类，可以派生出多个具体工厂类。   <br>每个具体工厂类只能创建一个具体产品类的实例。</blockquote>
    抽象工厂模式：
    <blockquote style="margin: 0 0 0 40px; border: none; padding: 0px;">多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。   <br>一个抽象工厂类，可以派生出多个具体工厂类。   <br>每个具体工厂类可以创建多个具体产品类的实例。  </blockquote>



