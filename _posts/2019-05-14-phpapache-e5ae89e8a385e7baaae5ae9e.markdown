---
author: lexiao
comments: true
date: 2019-05-14 01:35:52+00:00
layout: post
link: http://localhost/blog/?p=355
slug: phpapache-%e5%ae%89%e8%a3%85%e7%ba%aa%e5%ae%9e
title: php+apache 安装纪实
wordpress_id: 355
categories:
- 杂而无多
---

  1. 先安装 Apache。可以去  httpd.apache.org  下载。
    1. 安装过程中要提供domain name，一般是 localhost。
  2. 再安装php，安装过程中要提供  "[apacheInstallDir]/conf" 目录的路径。
  3. 安装完成后，要修改  "[apacheInstallDir]/conf/httpd.conf" 文件，以提供apache 服务器的正确参数，否则 server service会启动失败。
    1. 要修改  “Listen 。。。。” port，制定web server的端口，一般是选择  8080.  也可以写多行，每行指定一个端口。
    2. DocumentRoot  指定网页存放路径，如  “D:\Apache2.2\htdocs\index.html” 可以通过  “http://localhost:8080/index.html 访问。
    3. 安装php后，会在该文件中添加以下部分：
      1. a
      2. #BEGIN PHP INSTALLER EDITS - REMOVE ONLY ON UNINSTALL
      3. PHPIniDir "D:/Program Files (x86)/PHP"
      4. LoadModule php5_module "D:/Program Files (x86)/PHP/php5apache2_2.dll"
      5. #END PHP INSTALLER EDITS - REMOVE ONLY ON UNINSTALL
* a
* 其中，” PHPIniDir “ 和 ” LoadModule php5_module “  要提供正确的路径。
* 如果 apache web server service 启动失败，可以通过命令行方式debug。参见附录。

  


  


  


  


  


附录：

  


红色字体部分 要改为对应的  http service name 。可在httpd的管理monitor中查。

D:\Apache2.2\bin>**httpd.exe -w -n "Apache2.2" -k start**

httpd.exe: Syntax error on line 496 of D:/Apache2.2/conf/httpd.conf: Cannot load

D:/Apache2.2/php5apache2_2.dll into server: \xd5\xd2\xb2\xbb\xb5\xbd\xd6\xb8\xb

6\xa8\xb5\xc4\xc4\xa3\xbf\xe9\xa1\xa3

Note the errors or messages above, and press the <ESC> key to exit.  20...

  


D:\Apache2.2\bin>

D:\Apache2.2\bin>**httpd.exe -w -n "Apache2.2" -k start**

Syntax error on line 495 of D:/Apache2.2/conf/httpd.conf:

PHPINIDir takes one argument, Directory containing the php.ini file

Note the errors or messages above, and press the <ESC> key to exit.  15...

  


  


  


D:\Apache2.2\bin>

D:\Apache2.2\bin>**httpd.exe -w -n "Apache2.2" -k start**

httpd.exe: Could not reliably determine the server's fully qualified domain name

, using 192.168.1.7 for ServerName  这是正常启动。

  


D:\Apache2.2\bin>
