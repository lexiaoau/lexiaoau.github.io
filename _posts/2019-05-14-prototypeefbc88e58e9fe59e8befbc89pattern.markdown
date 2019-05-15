---
author: lexiao
comments: true
date: 2019-05-14 01:35:45+00:00
layout: post
link: http://localhost/blog/?p=351
slug: prototype%ef%bc%88%e5%8e%9f%e5%9e%8b%ef%bc%89pattern
title: Prototype（原型）pattern
wordpress_id: 351
categories:
- design pattern
---

  * 适用范围 
    1. 有大量的，在各方面都极为类似的product；product之间只有极小的差别（例如某个值，某个成员对象）。例如product为具体颜色。。。。。。
    2. 希望在system level减少子类的数量，并且不需要为每个product创建一个子类。 
    3. 者
  * 模式思路 
    1. abstract prototype 类声明一个“克隆”接口；每个concrete子类具体实现该“克隆”接口。 
    2. concrete子类在实现该“克隆”接口的时候，返回自身的一个已初始化的实例对象。在该返回过程中，在一个已经创建的object的基础上，修改有差别的值，复制其它无差别的成员，而生成一个新的本类的对象。
    3. 因为product的数量巨大，所以需要一个 Prototype Manager 类来注册和管理生成的 product。
  * 模式利弊 
    1. 利
      1. 可在 runtime动态指定加载的product类。
      2. 与工厂模式相比，可以大大减少子类数目，因为不需要为每一个product创建一个factory类。
      3. 通过改变object的值来指定新对象，而无需创建新类。
      4. 通过改变object的structure来指定新对象，而无需创建新类。
      5. 可以动态配置类。
    2. 弊
      1. 每个concrete类都需要实现“克隆”接口，对于一些现有的类可能会造成困难。
      2. 克隆所实现的应该是deep copy，而不是shallow copy。
  * 模式实现（实现是比较简单的，以下只讨论一些考虑因素） 
    1. prototype manager可以使用 hashtable 通过key来mapping具体的product。 
    2. 在克隆时，要注意指针类是否会出现循环引用的问题。
