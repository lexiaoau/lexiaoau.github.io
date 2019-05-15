---
author: lexiao
comments: true
date: 2019-05-14 01:37:11+00:00
layout: post
link: http://localhost/blog/?p=436
slug: preprocessor
title: PreProcessor
wordpress_id: 436
categories:
- C++
---

## 6.1 File Include Directive




两种格式： 


#include <filename> 格式一 


#include “filename” 格式二 











**格式一 **表示可以在系统的standard 目录中找到该文件。 





**格式二 **表示可以在系统的 alternative 目录中找到该文件。 


l 可以使用绝对路径名指定文件位置 


l 如果文件名前没有路径，则先查找用户的当前目录。如果没找到，再查找 standard 目录。 





如果被included 的文件中也有 file include directive， 则这些也会被先执行。 








## 6.2 Conditional Compilation




**#ifdef **


**#ifndef **


**#endif **








**#ifdef **和 **#ifndef** 都可以作为开始标志，而 **#endif **则是结束标志。 


在其间的语句会根据条件而决定是否被compile。 





这些语句可以嵌套使用。 





相关语句： 





**#define** 




