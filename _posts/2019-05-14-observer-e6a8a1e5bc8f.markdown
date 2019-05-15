---
author: lexiao
comments: true
date: 2019-05-14 01:36:51+00:00
layout: post
link: http://localhost/blog/?p=415
slug: observer-%e6%a8%a1%e5%bc%8f
title: Observer 模式
wordpress_id: 415
categories:
- design pattern
---

  * 适用范围 
    1. 当一个抽象关系中的各方存在依赖关系时，把每一方封装成为一个object，有助于改变、重用。  
    2. 当更改一个object会引起另一个object的更改时， 而且不知道多少个 object会被牵涉到。 
    3. 当一个object 想通知其它object，但是又不想知道其它object 是什么的情况下。（即 low couple）
    4. ---------定义了一种一对多的依赖关系,让多个观察者对象同时监听某一主题对象,在它的状态发生变化时,会通知所有的观察者
  * 模式思路 
    1. 首先定义 Subject（发出通知方） 和 Obect（接收通知方）。而且它们还可以分别被继承。 
    2. Object 向 Subject 注册，表示要求接收有关改动的通知。 
    3. 当 Subject 发生改动后，它通知所有注册了的 Object 。 
    4. Object 接收通知后，向 Subject 索取改动的具体信息，并改变 Object 自身中相关的部分。
  * 模式利弊 
    1. Subject 和 Object 实现了 low coupling。
      * 任何一方的改动不会影响另一方 
      * 每一方都可以被单独重用 
      * 配合设计的 layer 结构
    2. 采用的是“广播”方式，所以 Subject 不需要知道有多少个 Object 会接收，可以随时增加或者删除 Object；Object 接收消息后也可以决定处理还是不处理； 
    3. 一个坏处，由于 Subject 和 Object 互相不清楚对方的存在，所以不周全的设计可能导致一个“通知”被广泛传递（例如由一个 observer 再传给它下层的 observer，在 layer 结构中），有可能导致未预见到的后果。另外，如果 Object 无法知道 Subject 发生了什么改动（往往是协议没有设计好），那么它可能必须费很大功夫去找出这个改动是什么，才可以决定自己是否需要改动。所以，关于“通知消息”的协议必须设计好，和适用于layered 结构。
  * 模式实现（实现是比较简单的，以下只讨论一些考虑因素） 
    1. 如何让 Subject 知道它的相关 Object。  
      1. 可以在 Subject 里面保存一个 Object 的 ref list。  
      2. 可以在 Subject 里面保存一个 Object 的 hashtable
    2. 如果一个 Object 在 observe多个 Subject ，这时 Object 在索取 Subject 的 update时，Subject 应该告诉 Object 自己是“谁”，例如可以多传一个参数。 
    3. 由谁去发起通知？（即谁调用 Notify（） ）
      1. 由 Subject 在发生改变时自己调用。好处是简单，坏处是可能多个连续的改动触发了多个 notify（），而实际上只应该触发一次。 
      2. 由 client 在改变 Subject 时去调用。好处时可以在 Subject 的更改完成后，才触发 notify（）；坏处是 client 要多负起这个责任。
    4. 在删除一个 Subject 时，应该给其所有 Object 发一个通知，以便让 Object 在内部删除不需要的 ref。 
    5. 一个 Subject 可能会有多个方面（aspect）的变更，而一个 Object 可能只是对其中某个方面感兴趣。这种情况下，可以让 Object 在向 Subject 注册时，声明他是对那个（些）方面感兴趣。那么 Subject 以后就只会通知它这些方面的变更。 
    6. 当 Subject 和 Object 之间的依赖关系很复杂时，应该多加一层（Change Manager）来管理这些关系。Change Manager的责任是：
      1. 负责Subject 和 Object 之间的 mapping， 并提供接口来管理这个mapping 
      2. 定义update实现的规则 
      3. 当 Subject 发生变更时，它负责 update 所有相关的 Object
  


* 应用实例

  1. 比如ServletContextListener, 在applcation启动时, 会通知所有这个接口的实现类. 
