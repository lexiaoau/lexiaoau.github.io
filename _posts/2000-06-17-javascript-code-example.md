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

//// filter null
fundGroupNameArray = fundGroupNameArray.filter( x => x); 

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


///// ---------------------------------------------------------------------

module.exports.checkPeriodReached =  function( dateToCheck,  setDays, setSeconds  ) {    
    try {
        const dataDate = new Date( dateToCheck );
        const curTime = new Date();
        const diffTime =curTime - dataDate ;

        const dayPart = setDays * 24   * 60 * 60 * 1000 ;
        const secondsPart = setSeconds * 1000 ;
        const diffPeriod = dayPart + secondsPart ;

        if( diffTime >  diffPeriod  ) {
            return true;
        }
        else {
            return false;
        }
    } catch (err) {
       console.error('checkPeriodReached error:', err);
    }    
}



//////   设置为距今3个月之前的月份（跨年底也可以的）
 startDate.setMonth( currentDate.getMonth() - 3 );



//// set sleep
const sleep = async (milliseconds) => {
    return new Promise(resolve => setTimeout(resolve, milliseconds))
 }

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

---
---
---

## 使用 axios



```js


const axiosConfig = {
        url: getTokenApiUrl,
        method: 'post',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        data: {
            "CreateServiceToken_Request_1": {
                "ApplicationID": process.env.REUTERS_API_APPLICATION_ID
            }
        }
    }

    try {
        let rsp = await axios(axiosConfig);
        console.log(rsp["data"]);   
    }
    }
    catch (err) {
        console.log('Error caught', err);
    }


```


---
---
---

## 字符串及数字转换函数


```js

const ssecLast    = parseFloat(ssecData[2]);

// 数字 加 字符串，结果会变成字符串
// Math.round 是把小数部分删除，只保留整数部分
const ssecDiffRatio = (Math.round( ( (ssecCurrent - ssecLast) / ssecLast ) * 10000) / 100 )  + '';

// toLocaleString() 可以变成千位数用逗号分割的格式
const ssecCurrentPrice = ssecCurrent.toLocaleString();

// substring(5,7) 是截取 字符串中 第5和第6 字符。
const ssecMont = (ssecData[30]).substring(5,7);



```

---
---
---


## 正则表达式匹配


```js

// 'ASX listed companies as at Tue Sep 24 16:26:30 AEST 2019\n\nCompany name,ASX code,GICS industry 
//group\n"MOQ LIMITED","MOQ","Software & Services"\n"1300 SMILES LIMITED","ONT","Health Care Equipment & Services"\n
//"1414 DEGREES LIMITED","14D","Capital Goods"\n"1ST GROUP LIMITED","1ST","Health Care Equipment & Services"\n
//"333D LIMITED","T3D","Commercial & Professional Services"\n"360 CAPITAL GROUP","TGP","Real Estate"\n
//"360 CAPITAL TOTAL RETURN FUND","TOT","Not Applic"\n"3D OIL LIMITED","TDO","Energy"\n"3D RESO' 

        let reg = /,"([A-Z0-9]{3})",/gm ;

        // get all matches of patter, m - multiple line, g - match all instance
        let result = content.match(reg);
        
// result :
[ ',"MOQ",',
  ',"ONT",',
  ',"14D",',
  ',"1ST",',
  ',"T3D",',
  ',"TGP",',
  ',"TOT",',
  ',"TDO",' ]
  
  ////  只匹配8位数字，但是有前置匹配字符“<LipperId>”和后置匹配字符“<”。这样可以避免误中其它字符串。
  let reg = /(?<=<LipperId>)\d{8}(?=<)/gm ;

```

---
---
---

##  loop , copy pefromance


```js

while( length-- ) {}

// classical for with length
for (let p = 0, pLen = arrData.length; p < pLen; ++p) {}

// loop over object
for (let key in obj) {
  let value = obj[key];
  console.log(key, value);
}

// duplicate array
let newArray = oldArray.slice(  );


```


---
---
---


## 精准小数位数

把要保留的小数位乘以 10n 提升到整数位，然后用 Math.round 去掉小数部分，然后再除以 10n 再次变为小数。

```js
const szscDiffRatio = (Math.round( ( (szscCurrent - szscLast) / szscLast ) * 10000) / 100 )  + '';
```


## 类型转换

```js

//// 数值转为字符串
const numStr = num + ''; //// 与空字符串相加

///// 字符串转为数值
parseInt(), parseFloat()

```

## 获取深层次值的安全方法

```js 

//// 例如 reutersRawData.Charges.ChargesType[0].ChargesList.ChargesItem
module.exports.getDescendentValue =  function( obj, desc, defaultValue  ) {
    try {
        var arr = desc.split('.');
        while (arr.length>0) {
            let cur = arr.shift();
            //// handle array
            if( cur.includes('[')  )
            {
                let parts = cur.split('[') ;
                let fieldName = parts[0].trim() ;
                if ( obj[fieldName] ) {
                    obj = obj[fieldName] ;
                } else {
                    return defaultValue;
                }

                let numStr = (parts[1].split(']'))[0] ;
                numStr = Number(numStr) ;

                if( !Array.isArray(obj) || obj.length < (numStr + 1) ) {
                    return defaultValue;
                }
                obj = obj[numStr];
            }
            //// handle object
            else {
                if( obj[cur] ) {
                    obj = obj[cur];
                }
                else {
                return defaultValue;
                }
            }
        }
        return obj;
    } catch (err) {
        return defaultValue;
    }    
}

```













