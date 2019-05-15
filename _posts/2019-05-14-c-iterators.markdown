---
author: lexiao
comments: true
date: 2019-05-14 01:37:03+00:00
layout: post
link: http://localhost/blog/?p=427
slug: c-iterators
title: C++ Iterators
wordpress_id: 427
categories:
- C++
---


<table cellpadding="0" border="0" style="BORDER-COLLAPSE: collapse" cellspacing="0" >
<tbody >
<tr style="mso-yfti-irow: 0; mso-yfti-firstrow: yes" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


**Iterator**

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


**Description**

</td></tr>
<tr style="mso-yfti-irow: 1" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


input_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


Read values with forward movement. These can be incremented, compared, and dereferenced.

</td></tr>
<tr style="mso-yfti-irow: 2" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


output_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


Write values with forward movement. These can be incremented and dereferenced.

</td></tr>
<tr style="mso-yfti-irow: 3" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


forward_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


_Read or write_ values with _forward movement_. These combine the functionality of input and output iterators with the ability to store the iterators value.

</td></tr>
<tr style="mso-yfti-irow: 4" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


bidirectional_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


Read and write values with _forward and backward movement_. These are like the forward iterators, but you can increment and decrement them.

</td></tr>
<tr style="mso-yfti-irow: 5" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


random_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: #eeeeff; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


Read and write values with _random access_. These are the most powerful iterators, combining the functionality of bidirectional iterators with the ability to do pointer arithmetic and pointer comparisons.

</td></tr>
<tr style="mso-yfti-irow: 6; mso-yfti-lastrow: yes" >

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


reverse_iterator

</td>

<td style="BORDER-RIGHT: #ece9d8; PADDING-RIGHT: 4.8pt; BORDER-TOP: #ece9d8; PADDING-LEFT: 4.8pt; BACKGROUND: white; PADDING-BOTTOM: 4.8pt; BORDER-LEFT: #ece9d8; PADDING-TOP: 4.8pt; BORDER-BOTTOM: #ece9d8" >


_Either a random iterator or a bidirectional iterator_ that moves in reverse direction.

</td></tr></tbody></table>









  1. iterator 的 begin（） 和 end（）



    1. 每种容器都定义了 这一对 函数。


    2. 这两个函数返回的都是 iterator ，begin（） 指向 container 的 第一个元素，end（）指向 最后一个元素 **之后**。


    3. 如果 container 为空，那么 这两个函数返回的 iterator 相等。


  2. iterator 的操作



    1. 访问其所指向的元素———— 用 *（iterator） 来访问。（但是不能对 end（） 使用）


    2. ++ 和 -- 用以移动 iterator。（但是不能对 end（） 使用++）


    3. 比较 iterator 使用的是 “==” 和 “!=”——————如果指向同一个元素，则相等，否则不相等。


  3. const_iterator



    1. 这种 iterator 不能改变 被指向的元素 的值，适合用于 只读取元素 的情况下。


    2. 要注意区分 const_iterator ， const的 iterator ， const container 这三个用法。



      * const_iterator ———— 迭代器本身可以移动（改变），但是其指向的值不能改变，例如， vector<int>::const_iterator iter = nine.begin(); ++iter;


      * const的 iterator ————迭代器本身不可以移动（改变），但是其指向的值可以改变，例如 const vector<int>::iterator iter = nine.begin();


      * const container ———— container 里面的元素不能改变，例如 const vector<int> nines(10, 9)


  4. iostream 迭代器



    1. 通用注意



      1. 在生成 迭代器 时，必须绑定一个 流对象。


    2. istream 迭代器 



      1. 可以 用在 定义了 （>>）操作符的 类型上。


      2. istream 迭代器可以比较 （==）


    3. ostream 迭代器



      1. 可以 用在 定义了 （<<）操作符的 类型上。


      2. ostream 迭代器 不可以 比较


  5. reverse iterator 反向迭代器



    1. 反向迭代器与 正常迭代器 的移动方向 相反；例如，操作符“++”，在正常迭代器中从头至尾移动，而在反向迭代器中，是从尾至头移动；“——”亦是类似。


    2. 正常迭代器有 begin() , end() ； 而反向迭代器，是有 rbegin() , rend()。 除此之外，还有 base（）函数，它返回反向迭代器当前位置 右边 的一个元素。参见图。[![](http://img.blog.163.com/photo/JOAaWo0EHsSBWX8U8b2LqA==/2537215440070084630.jpg)](http://img.blog.163.com/photo/JOAaWo0EHsSBWX8U8b2LqA==/2537215440070084630.jpg)


    3. 把一个正常迭代器，赋值或者初始化给一个反向迭代器，则这两个迭代器并不一定指向同一个元素。


  6. 

