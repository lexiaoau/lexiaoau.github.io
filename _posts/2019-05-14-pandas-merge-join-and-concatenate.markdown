---
author: lexiao
comments: true
date: 2019-05-14 01:34:35+00:00
layout: post
link: http://localhost/blog/?p=313
slug: pandas-merge-join-and-concatenate
title: pandas Merge, join, and concatenate
wordpress_id: 313
categories:
- python
---

合并，联合以及连接





**Merge, join, and concatenate**





[http://pandas.pydata.org/pandas-docs/stable/merging.html](http://pandas.pydata.org/pandas-docs/stable/merging.html)









【





pd.concat(objs,
axis=0, join='outer', join_axes=None, ignore_index=False,





keys=None, levels=None, names=None,
verify_integrity=False,





copy=True)






 
  * objs : a
     sequence or mapping of Series, DataFrame, or Panel objects. If a dict is
     passed, the sorted keys will be used as the _keys_ argument, unless
     it is passed, in which case the values will be selected (see below). Any
     None objects will be dropped silently unless they are all None in which
     case a ValueError will be raised.

 
  * axis : {0,
     1, ...}, default 0. The axis to concatenate along.

 
  * join :
     {‘inner’, ‘outer’}, default ‘outer’. How to handle indexes on other
     axis(es). Outer for union and inner for intersection.

 
  * ignore_index
     : boolean, default False. If True, do not use the index values on the
     concatenation axis. The resulting axis will be labeled 0, ..., n - 1. This
     is useful if you are concatenating objects where the concatenation axis
     does not have meaningful indexing information. Note the index values on
     the other axes are still respected in the join.

 
  * join_axes
     : list of Index objects. Specific indexes to use for the other n - 1 axes
     instead of performing inner/outer set logic.

 
  * keys :
     sequence, default None. Construct hierarchical index using the passed keys
     as the outermost level. If multiple levels passed, should contain tuples.

 
  * levels : list
     of sequences, default None. Specific levels (unique values) to use for
     constructing a MultiIndex. Otherwise they will be inferred from the keys.

 
  * names :
     list, default None. Names for the levels in the resulting hierarchical
     index.

 
  * verify_integrity
     : boolean, default False. Check whether the new concatenated axis contains
     duplicates. This can be very expensive relative to the actual data
     concatenation.

 
  * copy :
     boolean, default True. If False, do not copy data unnecessarily.





】









frames = [ df1, df2,
df3 ]









· 
concat 的合并方向选项：





o 
1. axis = 0 (默认选项) ： 行跟着行





o 
2. axis = 1 ： 列跟着列









· 
concat 的合并交集方式：





o 
1. join='outer'
(默认选项)
： 取并集，以求不损失数据（信息）





o 
2. join='inner' ： 取交集，即所有df
中都存在的index





o 
3. join_axes=[df1.index] ： 以某个df的index范围进行判断，在里面的就合并，不在里面的就舍弃。













**Append**



























  













**Merge****（**根据列的值，而非index，来进行合并**）******









【









pd.merge(left, right, how='inner', on=**None**, left_on=**None**, right_on=**None**,





left_index=**False**, right_index=**False**, sort=**True**,





suffixes=('_x', '_y'), copy=**True**, indicator=**False**)










 
  * left:
     A DataFrame object

 
  * right:
     Another DataFrame object

 
  * on:
     Columns (names) to join on. Must be found in both the left and right
     DataFrame objects. If not passed and left_index and right_index are False, the intersection of the
     columns in the DataFrames will be inferred to be the join keys

 
  * left_on:
     Columns from the left DataFrame to use as keys. Can either be column names
     or arrays with length equal to the length of the DataFrame

 
  * right_on:
     Columns from the right DataFrame to use as keys. Can either be column
     names or arrays with length equal to the length of the DataFrame

 
  * left_index:
     If True,
     use the index (row labels) from the left DataFrame as its join key(s). In
     the case of a DataFrame with a MultiIndex (hierarchical), the number of
     levels must match the number of join keys from the right DataFrame

 
  * right_index:
     Same usage as left_index for
     the right DataFrame

 
  * how:
     One of 'left', 'right', 'outer', 'inner'. Defaults to inner. See below for more detailed
     description of each method

 
  * sort:
     Sort the result DataFrame by the join keys in lexicographical order.
     Defaults to True,
     setting to False will
     improve performance substantially in many cases

 
  * suffixes:
     A tuple of string suffixes to apply to overlapping columns. Defaults
     to ('_x', '_y').

 
  * copy:
     Always copy data (default True)
     from the passed DataFrame objects, even when reindexing is not necessary.
     Cannot be avoided in many cases but may improve performance / memory
     usage. The cases where copying can be avoided are somewhat pathological
     but this option is provided nonetheless.

 
  * indicator:
     Add a column to the output DataFrame called _merge with information on the
     source of each row. _merge is
     Categorical-type and takes on a value of left_only for observations whose merge
     key only appears in 'left' DataFrame, right_only for observations whose
     merge key only appears in 'right' DataFrame,
     and both if
     the observation’s merge key is found in both.









】





















· 
如何使用 how 参数例子：













· 
如何使用 indicator参数例子：









· 
**In [50]:
**pd.merge(df1, df2, on='col1', how='outer', indicator=True)





· 
Out[50]: 





· 
col1 col_left col_right 
_merge





· 
0 0 
a NaN left_only





· 
1 1 b 
2.0 both





· 
2 2 
NaN 2.0 right_only





· 
3 2 
NaN 2.0 right_only













**Join****（**从两个可能index不相同的df中，把部分列合并到一起**）******

















图1

















图2













图3
















