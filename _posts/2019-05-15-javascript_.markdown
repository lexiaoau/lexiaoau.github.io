---
author: lexiao
comments: true
date: 2019-05-15 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=449
published: true
slug: null
title: javascript 学习
wordpress_id: 449
categories:
- javascript
- 前端
---

## **DOM Selectors**

--------------  
getElementsByTagName  
getElementsByClassName  
getElementById  

querySelector  
querySelectorAll  

getAttribute  
setAttribute  

##Changing Styles  
style.{property} //ok  

className //best  
classList //best  

classList.add  
classList.remove  
classList.toggle  

##Bonus  
innerHTML //DANGEROUS  

parentElement  
children  

##It is important to CACHE selectors in variables  


-------------------------------------------------------------------------------------------------  

获取某个element的 CSS 属性值：  

var bodystyle = **getComputedStyle**(document.body);  
var b11 = bodyStyle**.getPropertyValue**("background");  

返回值是string  
