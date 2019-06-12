---
author: lexiao
comments: true
date: 2019-05-14 01:36:10+00:00
layout: post
link: http://localhost/blog/?p=375
slug: c%e4%b8%ad%e8%b0%83%e7%94%a8c%e5%87%bd%e6%95%b0
title: c++中调用c函数
wordpress_id: 375
categories:
- C++
---

因为C＋＋和C是两种完全不同的编译链接处理方式，所以如果直接在C＋＋里面调用C函数，这样链接起来是通不过的，会报链接错误，找不到函数体，所以要在C＋＋文件里面显示声明以下一些函数是C写的，要用C的方式来处理，这个在C＋＋设计初期就考虑到兼容性的问题，所以是可以解决的。  
比如用C写了A.h和A.c这两个文件，里面包括了void A_app（int）这样的函数，那么在需要调用这个函数的CPP文件里面，就需要显示声明一下了。

 

1.引用头文件前需要加上 extern “C”，如果引用多个，那么就如下所示  
**extern “C”  
{  
**#include “A.h”  
#include “B.h”  
#include “C.h”  
#include “D.h”  
**};**

 

然后在调用这些函数之前，需要将函数也全部声明一遍。

 

2.C++调用C函数的方法,将用到的函数全部重新声明一遍

 

**extern “C”  
{  
extern void A_app（int）;  
extern void B_app（int）;  
extern void C_app（int）;  
extern void D_app（int）;  
};  
**

 

  
extern "C"

 

{

 

#include "../Security Algorithm/MD5.h"

 

extern void BackDigest(unsigned char* dst,int length,unsigned char* digest);

 

};  


 

------------------------------------ 例子程序 ----------------------------------------------

 

 

恢复a.h文件为一般性C头文件,在aa.cpp文件中extern包含a.h头文件或函数.

 

a.h:

 

#ifndef __A_H  
#define __A_H  
int ThisIsTest(int a, int b);  
#endif

 

aa.cpp:

 

extern "C"  
{  
#include "a.h"  
}  
#include "aa.h"  
#include "stdio.h"

 

int AA::bar(int a, int b){   
printf("result=%d\n", ThisIsTest(a, b));   
return 0;  
}

 

or

 

aa.cpp:

 

#include "aa.h"  
#include "stdio.h"

 

extern "C"  
{  
int ThisIsTest(int a, int b);  
}

 

int AA::bar(int a, int b){   
printf("result=%d\n", ThisIsTest(a, b));   
return 0;  
}
