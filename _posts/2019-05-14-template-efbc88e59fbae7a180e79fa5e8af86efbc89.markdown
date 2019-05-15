---
author: lexiao
comments: true
date: 2019-05-14 01:36:58+00:00
layout: post
link: http://localhost/blog/?p=422
slug: template-%ef%bc%88%e5%9f%ba%e7%a1%80%e7%9f%a5%e8%af%86%ef%bc%89
title: template （基础知识）
wordpress_id: 422
categories:
- C++
---

如何才在可以在一个普通类中加入一个 _function template_ ？？？？










1、 template 的应用范围为： 函数 & 类。 即是有 _function template_ 和 _class template_ 。




2、 模板可以只声明而不定义。




3、 function template







    1. 格式：



**_template<typename_** T , **_typename_** T2 **_>_**




int compare( const **T** &v1, const **T** &v2 )




{ }










    2. 使用函数模板



int result = compare( 1, 3 ); //调用了compare( const **int**, const **int** )







    3. **inline** 函数模板



**_template<typename_** T> **_inline_** T min(const T&, const T&)







4、 class template




a) 格式：




**_template<typename_** T , **_typename_** T2 **_> //_****_模板在定义时有几个形_**




_**//参，在使用时就必须几个**_




class myQueue




{




public:




Queue();




………




};







b) 使用类模板




Queue< int, double> que;




Queue< vector<double>, string > que222;










5、 在模板内部定义 形参 的成员类型， 必须在 成员类型 前加上关键字 typename，以告诉编译器这是一个 成员类型，而不是一个数据成员。例如，




**_template<typename_** T , **_typename_** T2 **_>_**




T compare( const **T** &v1, const **T2** &v2 )




{




**_typename_** T2::size_type * p; //如果不加上**_typename_**，则后面的// //会被认为是一个数据成员




}




编写模板时，应该尽可能减少对 实参类型 的要求。例如，作比较的，不要一会用 小于，一会用 大于，如果全部都用 小于，这样就可以减少对 大于 操作符的依赖。









