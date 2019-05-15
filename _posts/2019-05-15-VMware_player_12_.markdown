---
author: lexiao
comments: true
date: 2019-05-15 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=453
published: true
slug: null
title: 使用VMware player 12 创建虚拟机
wordpress_id: 453
categories:
- vmware
---




  1. 指定工作目录
  2. 虚拟硬盘空间：20G。 内存：1G。
  3. 安装vmtools
    1. 找到菜单 player - manage - 安装 vmtools
    2. 在弹出窗口中，找到VMwareTools-xxx.tar.gz ，解压缩。
    3. 进入解压缩目录，然后 sudo ./vmware-install.pl
    4. 安装过程中一直按enter即可，或者输入【】提示yes和no，直到安装完毕。
    5. <blockquote><blockquote><blockquote>将Ubuntu关机（power off），否则不能添加共享文件夹</blockquote>
>>
>> </blockquote>
>
> </blockquote>

2. 在VMware虚拟机窗口，选择VM->Settings->Options->Shared Folders

3. 点右边的Add，点Next->选择Win7共享目录的路径，然后点Next->选中Enable this share->Finish

4. 在VM->Settings->Options->Shared Folders窗口的右边，Folder sharing栏里选择Always enabled

5. 点 OK 确定退出

    6. 共享目录在Ubuntu的路径是 /mnt/hgfs/vmshare/

  4. Ubuntu 下快速部署安装 Apache + PHP + MySQL + phpMyAdmin 笔记

    1. 更新安装管理器
    2. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">sudo apt-get update -y</code>

    3. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;"><strong style="background-color: white; border: 0px; box-sizing: border-box; color: #333333; font-family: "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Hiragino Sans GB W3", "WenQuanYi Micro Hei", "Microsoft YaHei UI", "Microsoft YaHei", sans-serif; font-size: 14px; margin: 0px; outline: 0px; padding: 0px; vertical-align: baseline; white-space: normal;">安装 Apache          </strong></code>sudo apt-get install apache2

    4. <blockquote style="border: none; margin: 0 0 0 40px; padding: 0px;"><br></br>

    <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">sudo systemctl start apache2.service</code>

</blockquote>

     5. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;"><strong style="background-color: white; border: 0px; box-sizing: border-box; color: #333333; font-family: "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Hiragino Sans GB W3", "WenQuanYi Micro Hei", "Microsoft YaHei UI", "Microsoft YaHei", sans-serif; font-size: 14px; margin: 0px; outline: 0px; padding: 0px; vertical-align: baseline; white-space: normal;">安装mysql</strong></code>

       1. **sudo apt-get install mysql-server -y**
       2. **Then, run sudo /usr/bin/mysql_secure_installation. Press "y". 这是进行安全设置。**
     6. **安装php**
       1. **

     <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">sudo apt-get install php -y</code>

 **
     7. 重启serveice
       1. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">sudo systemctl enable apache2.service<br></br>sudo systemctl enable mysql.service</code>

       2. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">

     <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;">systemctl restart apache2.service</code>

 `

       3. <code style="border-radius: 3px; border-width: 0px; box-sizing: border-box; font-family: Consolas, Menlo, Monaco, monospace, serif; padding: 0px;"><br></br></code>
