---
author: lexiao
comments: true
date: 2019-05-14 01:36:24+00:00
layout: post
link: http://localhost/blog/?p=387
slug: strategy-pattern
title: Strategy Pattern
wordpress_id: 387
categories:
- design pattern
---

* 适用范围

  1. 很多类似的class 只是在某些行为上有不同。
  2. 当需要不同的算法（logic）时
  3. 当算法使用某些client不应该知道的data时；使用该模式可以decouple两者的联系
  4. 一个类定义了很多条件下的行为。使用该模式可以改善design。
  5. ---------定义了算法家族, 分别封装起来, 让它们之间可以互相替换. 
* 模式思路

  1. 有一个 context 类，该类作为 client 与 state object 间的 双向 桥梁， 可以 从 client 向 state 传递 configuration，也可以把自身传递给 state，让 concrete state 在处理时可以访问 context（如果有必要的话）。
  2. state 是一个 类 hierachy，一个抽象父类，其每一个子类代表一个具体状态。所有类都实现共同的接口。
  3. Context 负责控制 state 的变化（即现在是进入那个 state class），然后把相应的request 交给对应的 state 类去处理 。
  

* 应用实例

  1. 比如Collections.sort(List list, Comparator c); 可以通过实现多个Comparator接口来达到多种排序的目的. 
