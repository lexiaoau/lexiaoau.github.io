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
tags: recent fe
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

arr = [
    {"device":"desktop","sessions":504,"reportmonth":"2019-01"},
{"device":"mobile","sessions":113,"reportmonth":"2019-01"},
{"device":"tablet","sessions":14,"reportmonth":"2019-01"},
{"device":"desktop","sessions":714,"reportmonth":"2019-02"},
{"device":"mobile","sessions":114,"reportmonth":"2019-02"},
{"device":"tablet","sessions":36,"reportmonth":"2019-02"},
{"device":"desktop","sessions":700,"reportmonth":"2019-03"},
{"device":"mobile","sessions":96,"reportmonth":"2019-03"},
{"device":"tablet","sessions":28,"reportmonth":"2019-03"},
{"device":"desktop","sessions":562,"reportmonth":"2019-04"},
{"device":"mobile","sessions":102,"reportmonth":"2019-04"},
{"device":"tablet","sessions":41,"reportmonth":"2019-04"},
{"device":"desktop","sessions":662,"reportmonth":"2019-05"},
{"device":"mobile","sessions":68,"reportmonth":"2019-05"},
{"device":"tablet","sessions":24,"reportmonth":"2019-05"}]

arr.filter(e => e.reportmonth === mon )
   .map(a => a.sessions )
   .reduce( (a,b) => a + b, 0 )


```

---



## 比较 timestamp



```js

// 生成 timestamp
const timeStamp = (new Date()).toISOString();

// 使用之前 生成 timestamp 来创建新的  Date object
const createTime = new Date(user.verificationCodeUpdatedAt);

// 与当前时间比较，比较的单位是 毫秒 ，以下代码判断是否大于 10 分钟。
const curTime = new Date();
(curTime - createTime) > 10 * 60 * 1000

```

---



## Node JS 如何 export/import 多个常量

```js

// file    utils/appConfigConst.js           export 常量
module.exports = {
    ReutersApiToken: 'token'
}

// file     utils/dbService.js                  import 和 使用常量
const AppConfigTableKey = require('./appConfigConst');
console.log(AppConfigTableKey.ReutersApiToken); 

```

---
---
---

## 提取环境变量

假设环境变量保存在文件 .env.docker-development
DB_PASSWORD=123

### 启动命令：    NODE_ENV=docker-development node utils/reutersService.js

```js
// 调用 环境变量

console.log(process.env.DB_PASSWORD）


```










