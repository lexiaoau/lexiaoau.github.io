---
author: lexiao
comments: true
date: 2019-05-14 01:35:44+00:00
layout: post
link: http://localhost/blog/?p=350
slug: '%e3%80%90zt%e3%80%91%e5%9c%a8xp%e5%92%8cwin7%e4%b8%8a%e6%b7%bb%e5%8a%a0ultraedit%e5%88%b0%e9%bc%a0%e6%a0%87%e5%8f%b3%e9%94%ae%e8%8f%9c%e5%8d%95'
title: 【zt】在XP和win7上添加UltraEdit到鼠标右键菜单
wordpress_id: 350
categories:
- tips备忘
---

如何在XP和win7系统下 添加UltraEdit到鼠标右键菜单

1、在注册表中【HKEY_CLASSES_ROOT\*\shell】下面新建一个项：Use_UltraEdit

2、在新建的项目中，再新建一个项：command

3、之后在【HKEY_CLASSES_ROOT\*\shell\Use_UltraEdit\command\】的默认值中，写入ultraedit的绝对路径，并添加%1

比如：D:\Program Files\UltraEdit\Uedit32.exe %1

然后退出注册表，找个文件用右键点击看看吧。
