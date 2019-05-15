---
author: lexiao
comments: true
date: 2019-05-14 01:36:29+00:00
layout: post
link: http://localhost/blog/?p=394
slug: use-case-diagram
title: Use Case Diagram
wordpress_id: 394
categories:
- 杂而无多
---

Use Case Diagram









  1. 一个 Use Case 的内容 


    * 触发（initiates）该 Use Case 的人/事件


    * Assumptions


    * Preconditions


    * 步骤


    * Postconditions


    * The actor who benefits from the use case


  2. Use Case 间的几种关系 


    1. inclusion ———— reuse one use case's steps inside another use case.


    2. extension ———— create a new use case by adding steps to an existing use case


    3. generalization ———— one use case inheriting from another.


    4. Grouping ———— organizing a set of use cases.


  3. Inclusion 的图形 


    1. Figure 1，见图中红色部分  
[![](http://img.blog.163.com/photo/SPLADynpqhWPSLx8tfoj0Q==/4291930444884382256.jpg)](http://img.blog.163.com/photo/SPLADynpqhWPSLx8tfoj0Q==/4291930444884382256.jpg)


  4. Extension的图形 


    1. Figure 2，见图中红色部分


    2. [![](http://img.blog.163.com/photo/my8tORi2N4JcVhYDz3BFgQ==/4291930444884382255.jpg)](http://img.blog.163.com/photo/my8tORi2N4JcVhYDz3BFgQ==/4291930444884382255.jpg)


  5. Use Case 在需求分析中的作用 


    1. Client interviews should start the process. These interviews will yield class diagrams that  
serve as the foundation for your knowledge of the system's domain (the area in which it will  
solve problems). After you know the general terminology of the client's area, you're ready to  
start talking to users.  
  
Interviews with users begin in the _**terminology of the domain**_ but should then shift into the  
_**terminology of the users**_. The _**initial results**_ of the interviews should reveal _**actors and highlevel  
use cases**_ that describe functional requirements in general terms. This information  
provides the boundaries and scope of the system.  
  
Later interviews with users delve into these requirements more closely, resulting in _**use case  
models that show the scenarios and sequences in detail**_. This might result in additional use  
cases that satisfy inclusion and extension relationships. In this phase it's important to rely on  
your understanding of the domain (from the class diagrams derived from client interviews). If  
you don't understand the domain well, you might create too many use cases and too much  
detail—a situation that could greatly impede design and development.  

