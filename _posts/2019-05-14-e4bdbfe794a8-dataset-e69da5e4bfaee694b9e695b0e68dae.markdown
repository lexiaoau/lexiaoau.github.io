---
author: lexiao
comments: true
date: 2019-05-14 01:36:30+00:00
layout: post
link: http://localhost/blog/?p=396
slug: '%e4%bd%bf%e7%94%a8-dataset-%e6%9d%a5%e4%bf%ae%e6%94%b9%e6%95%b0%e6%8d%ae'
title: 使用 DataSet 来修改数据
wordpress_id: 396
categories:
- SQL Server 和 .NET
---

使用 DataSet 来修改数据

 

 
  1. 为 DataTable 和 DataColumn 添加 约束
 
  2. 在 Table 中查找数据 
 
  3. 在 Table 中 filter，排序 
 
  4. 在 Table 中修改数据 
 
  5. 如何处理 Update Failure 
 
  6. 如何在 DataSet 中使用 Transaction 
   
  1. 为 DataTable 和 DataColumn 添加 约束     
    1. DataTable 的相关约束     
      1. Unique
 
      2. 主键
 
      3. 外键
 
    2. DataColumn 的相关约束     
      1. 是否可以接受 null 值
 
      2. 是否自增加 ID
 
      3. 文本最大长度
 
      4. 是否只读
 
      5. Unique
 
    3. 添加约束，会影响速度。所以，在调用 Fill（）函数前，应该把 EnforceConstraints 属性设成 false，调用后再设成 true。
 
    4. 所有约束是保存在一个 ConstraintCollection 对象中     
      1. 这个对象对应着 DataTable 的 Constraints 属性，
 
      2. 要添加一个 约束 到 Collection 中，可以调用它的 Add（） 函数。
 
    5. 添加主键     
      1. 对 DataSet 的影响：如果一个 DataTable是有主键的，那么即使调用 Fill（）函数多次（假设每次都获取这个 table 的数据），在 dataSet 中会每次调用后自动删除重复的行；否则，重复的行不会被删除。
 
      2. 使用 DataTable 的 属性来添加主键     
        1. 代码实例     
          1. DataColumn[] productsPrimaryKey = new **DataColumn[]**   
{   
productsDataTable.Columns["ProductID"]   
};  
productsDataTable.**PrimaryKey** = productsPrimaryKey;
 
        2. 代码解释     
          1. 先创建一个 **DataColumn[]** 数组来保存该 table 的 主键（可能有多列），然后把 table 的 **PrimaryKey** 属性设置成这个数组。
 
          2. 在设置 **PrimaryKey** 后，这个对应的 DataColumn 的 _**AllowDBNull**_ 和 **_Unique_** 会自动被设成 **_false_** 和 **_true_**。
 
      3. 使用 ConstraintCollection 的 Add（）函数来添加主键     
        1. 这个方法没有上面一个好，因为要逐个 主键 来添加，比较麻烦。
 
        2. 代码实例     
          1. myDataSet.Tables["Orders"].Constraints.Add  
(   
"Primary key constraint",   
myDataSet.Tables["Orders"].Columns["OrderID"],   
true  
);   
// 添加 Unique 约束的代码  
UniqueConstraint myUC = new UniqueConstraint(myDataTable.Columns["myColumn"]);  
myDataTable.Constraints.Add(myUC); 
 
    6. 添加外键     
      1. 代码实例     
        1. ForeignKeyConstraint myFKC = new ForeignKeyConstraint(   
myDataSet.Tables["Orders"].Columns["OrderID"],  // 外键中的 主导方  
myDataSet.Tables["Order Details"].Columns["OrderID"] // 外键中的 从属方  
);  
myDataSet.Tables["Order Details"].Constraints.Add(myFKC);
 
      2. 注意： 是 外键中的 从属方 的值 受到 主导方 的约束。
 
    7. 为 DataColumn 添加约束     
      1. 代码实例     
        1. DataColumn productIDDataColumn = myDataSet.Tables["Products"].Columns["ProductID"];  
productIDDataColumn._**AllowDBNull**_ = false;  
productIDDataColumn._**AutoIncrement**_ = true;  
productIDDataColumn._**AutoIncrementSeed**_ = -1;  
productIDDataColumn._**AutoIncrementStep**_ = -1;  
productIDDataColumn._**ReadOnly**_ = true;  
productIDDataColumn._**Unique**_ = true;  
myDataSet.Tables["Products"].Columns["ProductName"]._**MaxLength**_ = 40;
 
  2. 在 Table 中查找数据     
    1. 要查找某个特定主键的 dataRow，必须保证该 Table 是设置了 主键的。然后可以用以下代码进行查找：  
DataRow productDataRow = productsDataTable.Rows.Find("3");   
如果主键是由 多个 列组成，则可以通过以下代码查找：  
  
object[] orderDetails = new object[] { 10248, 11 };  
DataRow orderDetailDataRow = orderDetailsDataTable.Rows.Find(orderDetails); 
 
  3. 在 Table 中 filter，排序     
    1. 通过 DataTable 的各个重载的 Select（）函数  
_**DataRow[] Select()  
DataRow[] Select(string **_**filterExpression**_**)**_  
_**DataRow[] Select(string **_**filterExpression**_**, string **_**sortExpression**_**)**_  
_**DataRow[] Select(string **_**filterExpression**_**, string **_**sortExpression**_**, DataViewRowState **_**myDataViewRowState**_**) **_
 
    2. 代码实例     
      1. DataRow[] productDataRows = productsDataTable.Select("ProductID <= 5", "ProductID DESC", DataViewRowState.OriginalRows);
 
      2. 关于 **Expression** 如何规定，参见 [DataColumn.Expression ](http://msdn2.microsoft.com/en-us/library/system.data.datacolumn..aspx)（可以创建非常复杂的表达式）。
 
  4. 在 Table 中修改数据     
    1. 如何更新（Update）数据     
      1. 两种更新模式（不覆盖其他用户 & 覆盖）及其 使用     
        1. 模式一：不覆盖其他用户所做的修改     
          1. 模式规定：对某一行做更新时，如果发现它已经被其他用户修改过，则放弃自己所做的修改。
 
          2. 模式使用要求（以下要求同样适用于 DELETE 语句）：     
            1. 在 Update 语句中，必须包括 所有 在 SELECT 语句中的 列。
 
            2. 在修改数据之前，把值 设置成原来的值。
 
          3. 代码实例     
            1. myUpdateCommand.CommandText = "**_UPDATE_** Customers " +   
"SET CompanyName = @NewCompanyName,   
WHERE CompanyName = @OldCompanyName";  
  
myUpdateCommand.Parameters.Add("**@NewCompanyName**", SqlDbType.NVarChar, 40,  
"CompanyName");  
myUpdateCommand.Parameters.Add("**@OldCompanyName**", SqlDbType.NVarChar, 40, "CompanyName");  
myUpdateCommand.Parameters["@OldCompanyName"].**SourceVersion** = **DataRowVersion.Original**;   

 
            2. 在 **_Update_** 语句中，“Where”子句必须包括所有 在 SELECT 语句中的列，这满足模式第一个要求；要满足第二个要求，必须把 **SourceVersion** 设置成 **DataRowVersion.Original**
 
        2. 模式二：最后的修改才被写入数据库     
          1. 模式规定：对某一行做更新时，不管别人有没有修改，直接覆盖旧值。
 
          2. 模式使用要求（以下要求同样适用于 DELETE 语句）：     
            1. 在 WHERE 子句中（ Update 或者 DELETE)中，只需要包括主键。
 
    2. 如何添加 新行     
      1. 代码实例     
        1. DataRow myNewDataRow = **myDataTable.NewRow(); **  

myNewDataRow["CustomerID"] = "J5COM";  
myNewDataRow["CompanyName"] = "J5 Company";  
myNewDataRow["Address"] = "1 Main Street";

 

**myDataTable.Rows.Add**(myNewDataRow);

 

int numOfRows = **mySqlDataAdapter.Update**(myDataTable);

 
      2. 代码解释     
        1. 第一行是调用 DataTable 的 **NewRow()**来创建一个 新 row。
 
        2. 第二至四行，是为该行的各个列补充值。
 
        3. 第五行，调用 Rows 的 **Add**（） 把该列加到 行集合 中。
 
        4. 第六行，调用 Adapter 的 **Update（）** 函数 完成添加的操作。
 
    3. 如何修改 DataTable 中的 DataRow     
      1. 代码实例     
        1. myDataTable.**PrimaryKey** = new DataColumn[] { myDataTable.Columns["CustomerID"]  
DataRow myEditDataRow = myDataTable.Rows.**Find**("J5COM");   

myEditDataRow["CompanyName"] = "Widgets Inc.";  
myEditDataRow["Address"] = "1 Any Street";  
  
int numOfRows = mySqlDataAdapter.**Update**(myDataTable); 

 
      2. 代码解释     
        1. 在修改 DataRow 前，首先要确保 DataTable 已经设置了 **PrimaryKey** 。（注意：这一步通常应该是在 Adapter 调用了 Fill（）后做的 ）
 
        2. 使用 DataTable.Rows.**Find** 函数来找出符合条件的行。
 
        3. 对 DataRow 做出修改，
 
        4. 调用 Adapter 的 **Update****（）** 来把修改写入数据库。
 
      3. 对于修改 DataRow，可以调用 **BeginEdit()** 和 **EndEdit()**/ **CancelEdit()** 来标志 修改的开始 和 终结。
 
    4. 如何删除 DataTable 中的 DataRow     
      1. 与上述“如何修改 DataTable 中的 DataRow”，唯一不同的是在 **Find**之后，调用 DataRow 的 **Delete（）**来删除行。
 
    5. 与 修改 相关的 event
 
  5. 如何处理 Update Failure     
    1. 各种处理机制     
      1. 机制一：在发生错误时马上停止之后的所有操作。
 
      2. 机制二：发生错误后，仍然继续之后的无错误操作；在处理完成后，才整体地检查update过程中发生了什么错误。
 
    2. 机制二的实现     
      1. 代码实例     
        1. mySqlDataAdapter.**ContinueUpdateOnError** = true;  
  
if (**myDataSet.HasErrors**)  
{  
foreach (DataTable myDataTable in myDataSet.Tables)  
{  
if (**myDataTable.HasErrors**)  
{  
foreach (DataRow myDataRow in myDataTable.Rows)  
{  
if (**myDataRow.HasErrors**)  
{  
Console.WriteLine(**myDataRow.RowError**);  
foreach (DataColumn myDataColumn in myDataTable.Columns)  
{  
Console.WriteLine(myDataColumn + " original value = " +  
myDataRow[myDataColumn, DataRowVersion.Original]);  
Console.WriteLine(myDataColumn + " current value = " +  
myDataRow[myDataColumn, DataRowVersion.Current]);  
} } } } } } 
 
      2. 代码解释     
        1. 如果将 ContinueUpdateOnError 设置为 true，则在更新行过程中遇到错误时_**不引发异常**_。跳过该行的更新，并将错误信息置于出错行的 RowError 属性中。DataAdapter 继续更新后面的行。   

如果将 ContinueUpdateOnError 设置为 false，在更新行的过程中遇到错误时就会引发异常。

 
        2. DataSet，DataTable 和 DataRow 都有**HasErrors** 属性，以便检查是否在 update 过程中发生了错误。  

 
        3. **RowError** 是一个描述错误的字符串。
 
  6. 如何在 DataSet 中使用 Transaction     
    1. 代码实例     
      1. SqlTransaction mySqlTransaction = mySqlConnection.BeginTransaction();  
  
mySqlDataAdapter.SelectCommand.Transaction = mySqlTransaction;  
mySqlDataAdapter.InsertCommand.Transaction = mySqlTransaction;  
mySqlDataAdapter.UpdateCommand.Transaction = mySqlTransaction;  
mySqlDataAdapter.DeleteCommand.Transaction = mySqlTransaction;  
  
mySqlDataAdapter.Update(myDataSet);  
mySqlTransaction.Commit();
 
    2. 代码解释     
      1. 先建立一个 SqlTransaction 对象，然后把 它 传递给 各个有关的 Command（无关的应该不用吧）。
 
      2. 在 update之后，必须 Commit；否则，默认的 动作是 Rollback。
 
  7.  
  8. 
