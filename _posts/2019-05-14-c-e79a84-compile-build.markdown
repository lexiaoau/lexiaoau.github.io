---
author: lexiao
comments: true
date: 2019-05-14 01:37:08+00:00
layout: post
link: http://localhost/blog/?p=432
slug: c-%e7%9a%84-compile-build
title: C++ 的 compile & build
wordpress_id: 432
categories:
- C++
---

1、 在link 阶段时，所有 target 文件中用到的函数（包括构造函数）、变量等等，都必须在build出来的 .o 文件中有实现，否则会报“undefined reference”错误。







2、 对makefile做任何改动，会导致所有file 重新build 一次。




3、必须包含一个 main 函数。




4. static 变量必须在 cxx 文件中定义，例如 TypeXXX ClassXXX::var , 否则会出现 link failure(unresolve reference).
