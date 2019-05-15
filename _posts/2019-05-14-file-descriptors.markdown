---
author: lexiao
comments: true
date: 2019-05-14 01:36:44+00:00
layout: post
link: http://localhost/blog/?p=408
slug: file-descriptors
title: File Descriptors
wordpress_id: 408
categories:
- shell script
---



  1. 将 File Descriptors 与文件关联



    1. 例子：


    2. exec n>file  
exec n>>file （append到文件）


    3. exec n<file  
exec n<<file （append到文件）


  2. 重定向 output



    1. command 1> file1 2> file2 （重定向 STDOUT 和 STDERR )  
command > file 2>&1 （重定向 STDOUT 和 STDERR 到同一个文件)


  3. 特别技巧：利用 /dev/null 文件



    1. /dev/null 是系统中一个特殊文件，用来废弃 output。


    2. 废弃output ： command > /dev/null  
command > /dev/null 2>&1


    3. 删除文件内容： cat /dev/null > file
