---
author: lexiao
comments: true
date: 2019-05-14 01:36:31+00:00
layout: post
link: http://localhost/blog/?p=397
slug: '%e4%bd%bf%e7%94%a8-dataset-%e4%bf%9d%e5%ad%98%e6%95%b0%e6%8d%ae'
title: 使用 DataSet 保存数据
wordpress_id: 397
categories:
- SQL Server 和 .NET
---



  1. 在 DataSet 中如何调用 “存储进程”



    1. 代码实例



      1. // create a SqlCommand object and set its CommandText property  
// to call the CustOrderHist() stored procedure  
SqlCommand mySqlCommand = mySqlConnection.CreateCommand();  
mySqlCommand.CommandText = "EXECUTE CustOrderHist @CustomerID";  
mySqlCommand.Parameters.Add("@CustomerID", SqlDbType.NVarChar, 5).Value =  
"ALFKI";  
  
SqlDataAdapter mySqlDataAdapter = new SqlDataAdapter();  
mySqlDataAdapter.SelectCommand = mySqlCommand;   
DataSet myDataSet = new DataSet();  
mySqlConnection.Open();  
Console.WriteLine("Retrieving rows from the CustOrderHist() Procedure");  
int numberOfRows = mySqlDataAdapter.Fill(myDataSet, "CustOrderHist");  
Console.WriteLine("numberOfRows = " + numberOfRows);   
DataTable myDataTable = myDataSet.Tables["CustOrderHist"];  
foreach(DataRow myDataRow in myDataTable.Rows)  
{  
Console.WriteLine("ProductName = " + myDataRow["ProductName"]);  
Console.WriteLine("Total = " + myDataRow["Total"]);  
}


    2. 代码解释



      1. 首先是把 CommandText 设置成 执行一个 存储进程，其次是通过 Parameters.Add 传递该 存储进程 所需的参数


      2. 执行 命令，把返回的结果保存到一个 dataSet 中。然后通过一个 dataTable 就可以访问返回的结果。


  2. 一次取得多个 DataTable 的数据



    1. 代码实例



      1. // create a SqlCommand object and set its CommandText property  
// to mutliple SELECT statements  
SqlCommand mySqlCommand = mySqlConnection.CreateCommand();  
mySqlCommand.CommandText =   
"SELECT TOP 2 ProductID, ProductName, UnitPrice " +  
"FROM Products " +   
"ORDER BY ProductID;" +   
"SELECT CustomerID, CompanyName "  
+ "FROM Customers " +   
"WHERE CustomerID = 'ALFKI';";  
SqlDataAdapter mySqlDataAdapter = new SqlDataAdapter();  
mySqlDataAdapter.SelectCommand = mySqlCommand;  
DataSet myDataSet = new DataSet();  
mySqlConnection.Open();  
int numberOfRows = mySqlDataAdapter.Fill(myDataSet);  
Console.WriteLine("numberOfRows = " + numberOfRows);   
  
// change the TableName property of the DataTable objects  
myDataSet.Tables["Table"].TableName = "Products";  
myDataSet.Tables["Table1"].TableName = "Customers";  
foreach(DataTable myDataTable in myDataSet.Tables)  
{  
foreach(DataRow myDataRow in myDataTable.Rows)  
{  
foreach(DataColumn myDataColumn in myDataTable.Columns)  
{  
Console.WriteLine(myDataColumn + "= " + myDataRow[myDataColumn]);  
}  
}  
}


    2. 代码解释



      1. 首先是把 CommandText 设置成 执行 几个 select 语句，两个语句间以 分号 相隔，其次是通过 Parameters.Add 传递该 存储进程 所需的参数


      2. 执行 命令，把返回的结果保存到一个 dataSet 中。在本例中，因为有两个 select 语句，所以返回的结果是 两个 dataTable 的数据。因为在 中没有指定 table 名，所以会自动保存到名为“Table ” 和 “Table1” 的两个表中。


      3. 通过修改 TableName 属性，可以把“Table ” 和 “Table1” 重新命名为所希望的名字。


      4. 以上结果，也可以通过修改 CommandText 并分别执行两次（每次一个SELECT 语句），来达到。


  3. 可以用 多个 DataAdapter 来 fill 同一个 dataSet


  4. 可以通过 Merge 方法把 行、列、dataTable 合并到一个 现存的 dataSet 中。


  5. DataSet 与 XML 的操作



    1. 转换 DataSet 数据为 XML 格式： 使用 WriteXml（）函数


    2. 转换 DataSet 数据结构 为 XML 格式： 使用 WriteXmlSchema() 函数


    3. 从 XML 文件中读入数据： 使用 ReadXml（） 函数


  6. Table Mapping
