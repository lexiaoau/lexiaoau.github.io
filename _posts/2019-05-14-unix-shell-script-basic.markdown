---
author: lexiao
comments: true
date: 2019-05-14 01:35:51+00:00
layout: post
link: http://localhost/blog/?p=354
slug: unix-shell-script-basic
title: unix shell script basic
wordpress_id: 354
categories:
- shell script
---

  1. 特殊变量
    1. <table cellpadding="5" bgcolor="#999999" style="color: rgb(51, 51, 51); font-family: Arial; font-size: 14px; line-height: 26px; text-align: left; width: 622px;" border="1" ><tbody ><tr >
<td width="150" >**变量**
</td>
<td width="411" >**解释**
</td></tr><tr >
<td > $*
</td>
<td >展开为 "$1c$2c$3c$4c$5c...";其中字母c为变量 $IFS 的第一个字母, $IFS默认为空.
</td></tr><tr >
<td > $@
</td>
<td >展开为 "$1" "$2" "$3" "$4" "$5" ...
</td></tr><tr >
<td > $0
</td>
<td >当前脚本文件名
</td></tr><tr >
<td > $-
</td>
<td >脚本启动时传入的参数
</td></tr><tr >
<td > $#
</td>
<td >参数个数
</td></tr><tr >
<td > $?
</td>
<td >上一个命令的返回值
</td></tr><tr >
<td > $$
</td>
<td >当前脚本的进程ID(pid)
</td></tr><tr >
<td > $!
</td>
<td >上一个后台运行进程的进程号.
</td></tr><tr >
<td > $_
</td>
<td >上一个命令的最后一个参数.
</td></tr></table>
    2. d
  2. 流控制语句
    1. if -- else
      1. [http://xl2.blog.163.com/blog/static/17226383200711218367369/](http://xl2.blog.163.com/blog/static/17226383200711218367369/)
    2. 循环
      1. [http://xl2.blog.163.com/blog/static/17226383200711175215649/](http://xl2.blog.163.com/blog/static/17226383200711175215649/)
    3. 条件分支（case）
      1. [http://xl2.blog.163.com/blog/static/172263832007112194720201/](http://xl2.blog.163.com/blog/static/172263832007112194720201/)
    4. 文件、字符串、数字的比较和判断
      1. [http://xl2.blog.163.com/blog/static/172263832007112191626726/](http://xl2.blog.163.com/blog/static/172263832007112191626726/)
    5. sf
  3. 命令替换
    1. 例子：
      1. Lines=`wc -l textFile`  #使用 back tick ``` 来表示命令替换结果
      2. Lines=”$(wc -l textFile)”  #使用 $(CMD)  来表示命令替换结果
      3. 上述两个例子都将 wc 的结果保存到变量 Lines 中。
