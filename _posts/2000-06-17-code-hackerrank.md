---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: Hacker algo code example
wordpress_id: 440
tags: recent fe
categories:
- code
---




### Operators submission

There are  lines of numeric input:
- The first line has a double,  (the cost of the meal before tax and tip).
- The second line has an integer,  (the percentage of  being added as tip).
- The third line has an integer,  (the percentage of  being added as tax).


```python

# Complete the solve function below.
def solve(meal_cost, tip_percent, tax_percent):
    total = meal_cost * ( 1 + tip_percent / 100 + tax_percent / 100 )
    dif = 1 / 2
    intPart = int(total)
    total = intPart if total < intPart + dif else (intPart+1)              // 内置 round() 函数不是完全的四舍五入，所以要如此处理
    print(total)
    return total


```























