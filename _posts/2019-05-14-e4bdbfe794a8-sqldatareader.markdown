---
author: lexiao
comments: true
date: 2019-05-14 01:36:32+00:00
layout: post
link: http://localhost/blog/?p=398
slug: '%e4%bd%bf%e7%94%a8-sqldatareader'
title: 使用 SqlDataReader
wordpress_id: 398
categories:
- SQL Server 和 .NET
---



  1. 使用 列序号 来访问 列（好处是可以加快访问速度） 


    1. 代码实例 


      1. int productIDColPos = productsSqlDataReader.GetOrdinal("ProductID");   
Console.WriteLine(productsSqlDataReader[productIDColPos]);


    2. 代码解释 


      1. 第一行的用途是通过 reader 的 GetOrdinal（） 函数，获得某个列的序号； 

      2. 获得序号后，就可以像第二行那样，通过序号访问 列中数据


  2. 在使用完 reader 后，要关闭它，否则其相连的 connection 无法执行其它操作（如close）。 

  3. 要把数据库中的数据，转换成 .NET 中的数据类型，可以使用 reader.Get*() 函数（* 是一种 .NET 类型）。该种函数的使用，需要先获取列序号。 

  4. 使用 reader.GetFieldType（） 函数可以获取某个列的 “类型”信息（返回类型是“Type”）。类似的函数还有 GetDataTypeName() 。 

  5. 使用 reader 的 .GetSql* 函数 


    1. 代码实例 


      1. SqlInt32 productID = productsSqlDataReader.GetSqlInt32(productIDColPos); Console.WriteLine("productID = " + productID);


    2. 使用这种方法，速度比 Get* 函数更快。


  6. 处理 null 值 


    1. Sql* Type 不可以保存 null 类型，而 .NET 类型可以 

    2. 可以使用 IsDBNull(） 函数来检查某列是否为 null 。 

    3. 每个 Sql* Type 有一个 IsNull 属性也可以检查。


  7. 一次过执行多个 SQL 语句 


    1. 代码实例 


      1. SqlCommand mySqlCommand = mySqlConnection.CreateCommand();   
  
// set the CommandText property of the SqlCommand object to   
// the mutliple SELECT statements   
mySqlCommand.CommandText = "SELECT TOP 5 ProductID, ProductName "   
+ "FROM Products "   
+ "ORDER BY ProductID;"   
+ "SELECT TOP 3 CustomerID, CompanyName "   
+ "FROM Customers "   
+ "ORDER BY CustomerID;"   
+ "SELECT TOP 6 OrderID, CustomerID "   
+ "FROM Orders "   
+ "ORDER BY OrderID;";   
mySqlConnection.Open();   
SqlDataReader mySqlDataReader = mySqlCommand.ExecuteReader();   
  
// read the result sets from the SqlDataReader object using   
// the Read() and NextResult() methods   
do   
{   
while (mySqlDataReader.Read())   
{   
Console.WriteLine("mySqlDataReader[0] = " + mySqlDataReader[0]);   
Console.WriteLine("mySqlDataReader[1] = " + mySqlDataReader[1]);   
}   
Console.WriteLine("");   
// visually split the results   
} while (mySqlDataReader.NextResult()); 

      2. 代码解释 


        1. 在 CommandText 中包含多个 sql 语句，每个语句间以“；”分隔 

        2. 每个 SELECT 语句返回的结果会放到 一个 result set中，在本例中，reader 就包含了 3 个返回的 result set。要访问 reader中的下个 result set，可以调用 NextResult() 函数。 

        3. 访问当前 result set 中的行，可以用 Read（）函数。
