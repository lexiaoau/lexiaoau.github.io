---
author: lexiao
comments: true
date: 2019-05-14 01:34:36+00:00
layout: post
link: http://localhost/blog/?p=316
slug: python-unicode
title: Python Unicode
wordpress_id: 316
categories:
- python
---

### py文件中的编码




Python 默认脚本文件都是 `ANSCII` 编码的，当文件 中有非 ANSCII 编码范围内的字符的时候就要使用"`编码指示`"来修正一个 module 的定义中，如果.py文件中包含中文字符（严格的说是含有非anscii字符），则需要在第一行或第二行指定编码声明：`# -*- coding=utf-8 -*-` 或者 `#coding=utf-8`  
其他的编码如：gbk、gb2312也可以；

  
  


### python中的编码与解码




先说一下python中的字符串类型，在python中有两种字符串类型，分别是 `str` 和 `unicode`，他们都是basestring的派生类；






  * str类型是一个包含Characters represent (at least) 8-bit bytes的序列；


  * unicode 的每个 unit 是一个 unicode obj;




在str的文档中有这样的一句话：




<blockquote>The string data type is also used to represent arrays of bytes, e.g., to hold data read from a file.
> 
> </blockquote>




也就是说在读取一个文件的内容，或者从网络上读取到内容时，保持的对象为str类型；如果想把一个str转换成特定编码类型，需要把str转为Unicode,然后从unicode转为特定的编码类型如：utf-8、gb2312等。

  
  
  
  


建立一个文件test.txt，文件格式用ANSI，内容为:"abc中文",用python来读取



    
    <code><span># coding=gbk</span>
    <span>print</span> <span>open</span>(<span>"Test.txt"</span>).<span>read</span>()</code>
    
    <code><br></code>
    
    <code><p>把文件格式改成UTF-8：<br>结果：abc涓 枃</p>
    <p>显然，这里需要解码：</p>
    
    
    <code><span># coding=gbk</span>
    import codecs
    <span>print</span> <span>open</span>(<span>"Test.txt"</span>).<span>read</span>().decode(<span>"utf-8"</span>)</code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code>简单地说，<strong>python中的print直接把字符串传递给操作系统，</strong></code>
    
    <code><strong>所以你需要把str解码成与操作系统一致的格式</strong>。Windows使用CP936(几乎与gbk相同)，所以这里可以使用gbk。</code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><br></code>
    
    <code><h3>python 编码 检测</h3>
    <p>使用 <code>chardet</code> 可以很方便的实现字符串/文件的编码检测,例子如下:</p>
    
    
    <code><span>>></span>>import urllib
    <span>>></span>>rawdata = urllib.urlopen(<span>'http://www.google.cn/'</span>).read()
    <span>>></span>>import chardet
    <span>>></span>>chardet.detect(rawdata)
    {<span>'confidence'</span><span>:</span> <span>0</span>.<span>98999999999999999</span>, <span>'encoding'</span><span>:</span> <span>'GB2312'</span>}</code>




在工作中，经常遇到，读取一个文件，或者是从网页获取一个问题，明明看着是gb2312的编码，可是当使用decode转时，总是出错，这个时候，可以使用decode('gb18030')这个字符集来解决，如果还是有问题，这个时候，一定要注意，decode还有一个参数，比如，若要将某个 String对象s从gbk内码转换为UTF-8，可以如下操作  
`s.decode('gbk').encode('utf-8′)`  
可是，在实际开发中，我发现，这种办法经常会出现异常： 




<blockquote>UnicodeDecodeError: ‘gbk' codec can't decode bytes in position 30664-30665: illegal multibyte sequence
> 
> </blockquote>




这是因为遇到了非法字符——尤其是在某些用C/C++编写的程序中，**_全角空格_**往往有多种不同的实现方式，比如`\xa3\xa0`，或者`\xa4\x57`，这些字符，看起来都是全角空格，但它们并不是“合法”的全角空格（真正的全角空格是`\xa1\xa1`），因此在转码的过程中出现了异常。这样的问题很让人头疼，因为只要字符串中出现了一个非法字符，整个字符串——有时候，就是整篇文章——就都无法转码。 解决办法：  
`s.decode('gbk', ‘ignore').encode('utf-8′)`  
因为decode的函数原型是 `decode([encoding], [errors='strict'])`，可以用第二个参数控制错误处理的策略，默认的参数就是strict，代表遇到非法字符时抛出异常； 






  * 如果设置为ignore，则会忽略非法字符； 


  * 如果设置为replace，则会用?取代非法字符； 


  * 如果设置为xmlcharrefreplace，则使用XML的字符引用。 
  
  


文／hxzqlh（简书作者）  
原文链接：http://www.jianshu.com/p/53bb448fe85b  
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。

  


  


  


  


  


  


  


  


  


  


  


  


  


  


----------------------------------------------------------------------------------------------

  


  


  


  


  


为什么从 unicode 转 str 是 encode，而反过来叫 decode?




因为 Python 认为 16 位的 unicode 才是字符的唯一内码，而大家常用的字符集如 
gb2312，gb18030/gbk，utf-8，以及 ascii 都是字符的二进制（字节）编码形式。把字符从 unicode 
转换成二进制编码，当然是要 encode。

  


  


  


  

    
    <code style="line-height: 28px;"><div style="line-height: 28px;">----------------------------------------------------------------------------------------------</div><div style="line-height: 28px;"><br style="line-height: 28px;"></div><div style="line-height: 28px;"><br></div><div style="line-height: 28px;">
    
    str  -> decode('the_coding_of_str') -> unicode
    unicode -> encode('the_coding_you_want') -> str
    
    <br>
    
    <br>
    
    <br>

  


  


  


处理流程，可以这么使用，把python看做一个水池，一个入口，一个出口




入口处，全部转成unicode, 池里全部使用unicode处理，出口处，再转成目标编码

(当然，有例外，处理逻辑中要用到具体编码的情况)

`

  


  




    
    读文件
    
    外部输入编码，decode转成unicode
    
    处理(内部编码，统一unicode)
    
    encode转成需要的目标编码
    
    写到目标输出(文件或控制台)
    




  


  


  


  


  


  


若头部声明coding=utf-8, a = '中文' 其编码为utf-8




若头部声明coding=gb2312, a = '中文' 其编码为gbk

  


  


<blockquote>

> 
> 处理顺序
> 
> 
</blockquote>



    
    1. Decode early
    2. Unicode everywhere
    3. Encode later
    
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>
    
    <br>

  


  


  


  


  


`

`

  

