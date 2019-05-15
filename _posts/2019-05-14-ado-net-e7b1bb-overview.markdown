---
author: lexiao
comments: true
date: 2019-05-14 01:36:38+00:00
layout: post
link: http://localhost/blog/?p=403
slug: ado-net-%e7%b1%bb-overview
title: ADO.NET 类 Overview
wordpress_id: 403
categories:
- SQL Server 和 .NET
---



  1. ADO.NET 提供两大类数据库连接：Managed Provider（保持不断开的数据库连接），Generic Data（取得数据后断开的连接）。


  2. Managed Provider



    1. 类图 [![](http://img.blog.163.com/photo/BdMQemIws0BGtx6tIslC8g==/1425389282063281258.jpg)](http://img.blog.163.com/photo/BdMQemIws0BGtx6tIslC8g==/1425389282063281258.jpg)


    2. 目前有支持 3 种 数据库的 provider



      1. SQL SERVER（namespace：System .Data.SqlClient）


      2. OLE DB（例如 Oracle 或者 Access）（namespace：System.Data.OleDb）


      3. ODBC（比前两者慢很多，一般不采用）（namespace：System.Data.Odbc）


    3. Connection 类： 用以建立数据库连接


    4. Command 类： 用以运行 SQL 命令（SELECT, DELETE, UPDATE, INSERT)，存储进程。


    5. Pamameter 类：用以存储 要传送给 Command object 的参数。可以是 SQL 命令的参数。


    6. PamameterCollection 类： 是多个 Pamameter object 的集合。


    7. DataReader 类：用以读取返回的数据库行。只能前向阅读，但是不能修改数据。不过读的速度比 DataSet要快。


    8. DataAdapter 类： 一般是把数据库中的行读取到 dataSet 中，进行修改，然后用 DataAdapter 把这些修改 同步 回数据库中。


    9. CommandBuilder 类： 用以 为 对数据作出的修改 自动生成一条 SQL 语句。


    10. Transaction 类：用以建立一个 transaction，方便 rollback。


  3. Generic Data



    1. Generic Data 是把数据库中的数据取出，并且在本地保存一个 copy，从而可以对这些数据 排序、过滤、搜索 等等。并且最终可以通过 DataAdapter 把所做的修改 同步回 数据库中。


    2. 类图 [![](http://img.blog.163.com/photo/oFU2wCQSEw8dio4Uw1uqsw==/4514295676485896410.jpg)](http://img.blog.163.com/photo/oFU2wCQSEw8dio4Uw1uqsw==/4514295676485896410.jpg)


    3. DataSet 类： 可以表现数据库中的表、列、行等结构，还可以加主键、外键等约束。实际上，dataSet中数据是以 XML 格式保存的。


    4. DataTable 类


    5. DataRow 类


    6. DataColumn 类：


    7. Constrain 类： 一个 DataColumn 对应一个或者几个 Constrain。 一个 DataTable 包含多个 Constrain。


    8. DataView 类： 用来表现 指定的 某些 rows 。


    9. DataRelation 类： 用来表现 两个 tables 之间的关系。一个 DataSet 中可以包含多个 DataRelation 。


    10. UniqueConstraint 类： 用以表明 某个DataColumn object 中保存的值必须是 unique 的。


    11. ForeignKeyConstraint


    12. 

    13. 
