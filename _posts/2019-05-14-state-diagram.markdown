---
author: lexiao
comments: true
date: 2019-05-14 01:36:28+00:00
layout: post
link: http://localhost/blog/?p=393
slug: state-diagram
title: State Diagram
wordpress_id: 393
categories:
- 杂而无多
---

State Diagram









  1. State Diagram 的基本元素 


    1. 状态起始点——实心圆圈


    2. 状态终止点——实心圆圈外加一个圆


    3. 状态名称——如下图，写在状态方框中上部。


    4. 状态中的活动——如下图，写在状态方框中下部。常见活动可归为三类：1）entry；2）do；3）exit。


    5. trigger event——触发状态转换的事件，通常写在状态连接线的旁边


    6. Guard Condition——某种会触发状态转换的条件（例如电脑的屏保触发）。Guard Condition以方括号括起“[]”。


    7. 图1：  
[![](http://img.blog.163.com/photo/f2we4BqAwGFARxfJApHMWQ==/1440307455828754833.jpg)](http://img.blog.163.com/photo/f2we4BqAwGFARxfJApHMWQ==/1440307455828754833.jpg)


  2. State Diagram 的高级元素 


    1. Substates （states reside within a state） 



      1. 可以分为以下两类


      2. sequential Substates —— 按顺序发生的 Substates；见图中红色方框部分


      3. Concurrent Substates ——同时发生的 Substates；在图形中以点虚线分隔，见图中黄色方框部分；


    2. History States —— a composite state remembers its active substate when the object transitions out of the composite state.在图形中以H 加圆圈表示，见图中紫色部分。


    3. 图2：  
[![](http://img.blog.163.com/photo/sCYBzwTbDgSkCQTb03uGQw==/5362942731268425488.jpg)](http://img.blog.163.com/photo/sCYBzwTbDgSkCQTb03uGQw==/5362942731268425488.jpg)  

