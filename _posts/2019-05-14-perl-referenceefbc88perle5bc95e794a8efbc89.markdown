---
author: lexiao
comments: true
date: 2019-05-14 01:35:05+00:00
layout: post
link: http://localhost/blog/?p=338
slug: perl-reference%ef%bc%88perl%e5%bc%95%e7%94%a8%ef%bc%89
title: Perl reference（Perl引用）
wordpress_id: 338
categories:
- perl
---

  1. 创建引用
    1. 例子：
      1.       2. $scalarref = \$foo;
      3. $constref = \186_282.42;
      4. $arrayref = \@ARGV;
      5. $hashref = \%ENV;
      6. $coderef = \&handler;
      7. $globref = \*STDOUT;
    2. 匿名引用（anonymous）
      1. 匿名数组：使用“【 】”符号创建，例子：
        1. $arrayref = [1, 2, ["a", "b", "c", "d"]];
        2. 解释：创建了引用  “arrayref”， 指向一个匿名3元素数组，最后一个元素是一个新的匿名引用，指向一个4元素的数组。$arrayref–>[2][1]would have the value “b”.
      2. 匿名哈希：使用“{ }”符号创建，例子：
        1.           1. $hashref = {
          2. "Adam" => "Eve",
          3. "Clyde" => $bonnie,
          4. "Antony" => "Cleo" . "patra",
          5. };
        2. 要注意与其它场合的{} 区分（如block，正常哈希），
          1. 例子：如果希望在函数里面返回一个匿名哈希，那么：
            1.             2. sub hashem { { @_ } } # Silently WRONG — returns @_.
            3. sub hashem { +{ @_ } } # Ok.
            4. sub hashem { return { @_ } } # Ok.
  2. 使用引用
    1. 例子1：
      1.       2. $foo = "three humps";
      3. $scalarref = \$foo; 			# $scalarref is now a reference to $foo
      4. $camel_model = $$scalarref; 			# $camel_model is now "three humps"
* 例子2：

  1.   2. push(@$arrayref, $filename);
  3. $$arrayref[0] = "January"; 		# Set the first element of @$arrayref
  4. @$arrayref[4..6] = qw/May June July/; 		# Set several elements of @$arrayref
  5. %$hashref = (KEY => "RING", BIRD => "SING"); 		# Initialize whole hash
  6. $$hashref{KEY} = "VALUE"; 		# Set one key/value pair
  7. @$hashref{"KEY1","KEY2"} = ("VAL1","VAL2"); 		# Set two more pairs

<blockquote>  
</blockquote>
