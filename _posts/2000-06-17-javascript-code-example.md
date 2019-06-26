---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: JavaScript 之 code example
wordpress_id: 440
categories:
- javascript
- 前端
---

# code example



## filter, map , reduce

### 解释：
- arr 是array of objects ， 
- arr.filter(e => e.reportmonth === mon )  是寻找等于某个月的数组元素
- .map(a => a.sessions ) 是垂直提取 同类的 对象属性 组成新数组， sessions 是属性名字
- .reduce( (a,b) => a + b, 0 )  是对数组元素进行求和

```js

arr = [{"device":"desktop","sessions":504,"reportmonth":"2019-01"},{"device":"mobile","sessions":113,"reportmonth":"2019-01"},{"device":"tablet","sessions":14,"reportmonth":"2019-01"},{"device":"desktop","sessions":714,"reportmonth":"2019-02"},{"device":"mobile","sessions":114,"reportmonth":"2019-02"},{"device":"tablet","sessions":36,"reportmonth":"2019-02"},{"device":"desktop","sessions":700,"reportmonth":"2019-03"},{"device":"mobile","sessions":96,"reportmonth":"2019-03"},{"device":"tablet","sessions":28,"reportmonth":"2019-03"},{"device":"desktop","sessions":562,"reportmonth":"2019-04"},{"device":"mobile","sessions":102,"reportmonth":"2019-04"},{"device":"tablet","sessions":41,"reportmonth":"2019-04"},{"device":"desktop","sessions":662,"reportmonth":"2019-05"},{"device":"mobile","sessions":68,"reportmonth":"2019-05"},{"device":"tablet","sessions":24,"reportmonth":"2019-05"}]

arr.filter(e => e.reportmonth === mon ).map(a => a.sessions ).reduce( (a,b) => a + b, 0 )


```

---
















