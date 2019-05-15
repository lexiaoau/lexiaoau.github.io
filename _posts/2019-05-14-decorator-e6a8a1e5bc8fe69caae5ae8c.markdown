---
author: lexiao
comments: true
date: 2019-05-14 01:36:17+00:00
layout: post
link: http://localhost/blog/?p=382
slug: decorator-%e6%a8%a1%e5%bc%8f%e6%9c%aa%e5%ae%8c
title: Decorator 模式(未完)
wordpress_id: 382
categories:
- design pattern
---



  * 适用范围 



    1. 希望在 runtime 时为 “某个object ” 添加其它功能，但是不影响整个类（的其它 object ）。本模式的方法是把 该object ，封装到另外一个 外包object中（decorator object）。


    2. 当被额外添加的功能是不能 撤销（withdraw） 时。 


    3. 当不适合用继承来添加功能时。比如，会引起类膨胀（explosion of subclasses），或者是 类定义 不确定的时候。


  * 模式思路 



    1. 把 目标object ，封装到另外一个 外包object中（decorator object）。 


    2. 外包object 遵守 目标object 的接口约定 。 


    3. 当 client 调用 外包object 的接口时（实际上 client是想调用 目标object 的接口）， 外包object 做一些额外处理（额外功能） 后，把该 request 转发给 目标object 。


  * 模式利弊 



    1. 比起 继承方式，更具弹性。可以在程序运行任何时候 添加/删除 额外功能，可以 添加任意（层次）需要的功能。不需要创建过多的父类或者子类。另外，同一功能可以添加多次；而如果多次继承一个类的话，可能容易出错。


    2. 即需即装。使用  Decorator ，可以仅在需要时才添加额外功能，而无需预先定义一个“big” 类。


    3. 一个坏处，会导致很多小规模、接口相似的 object。对于后期维护，可能难以理解和debug。


  * 模式实现（实现是比较简单的，以下只讨论一些考虑因素） 



    1. 外包object 遵守 目标object 的接口约定 。


    2. 当只添加一个功能时，可以省略 abstract Decorator class。 


    3. 由谁去发起通知？（即谁调用 Notify（） ）



      1. 由 Subject 在发生改变时自己调用。好处是简单，坏处是可能多个连续的改动触发了多个 notify（），而实际上只应该触发一次。 


      2. 由 client 在改变 Subject 时去调用。好处时可以在 Subject 的更改完成后，才触发 notify（）；坏处是 client 要多负起这个责任。


    4. 在删除一个 Subject 时，应该给其所有 Object 发一个通知，以便让 Object 在内部删除不需要的 ref。 


    5. 一个 Subject 可能会有多个方面（aspect）的变更，而一个 Object 可能只是对其中某个方面感兴趣。这种情况下，可以让 Object 在向 Subject 注册时，声明他是对那个（些）方面感兴趣。那么 Subject 以后就只会通知它这些方面的变更。 


    6. 当 Subject 和 Object 之间的依赖关系很复杂时，应该多加一层（Change Manager）来管理这些关系。Change Manager的责任是：



      1. 负责Subject 和 Object 之间的 mapping， 并提供接口来管理这个mapping 


      2. 定义update实现的规则 


      3. 当 Subject 发生变更时，它负责 update 所有相关的 Object
