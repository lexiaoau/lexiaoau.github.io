---
author: lexiao
comments: true
date: 2019-09-04 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: typescript-学习笔记
wordpress_id: 440
tags: recent
categories:
- 前端
- typescript
---


# 如何定义 mapStateToProps

---

<table style="height: 238px;" width="100%">
<tbody>
<tr style="height: 18px;">
<td style="width: 11%; height: 18px;">&nbsp;</td>
<td style="width: 18%; height: 18px;">&nbsp;</td>
<td style="width: 10%; height: 18px;">&nbsp;</td>
<td style="width: 43%; height: 18px;">&nbsp;</td>
<td style="width: 7%; height: 18px;">&nbsp;</td>
<td style="width: 0.541272%; height: 18px;">&nbsp;</td>
</tr>
<tr style="height: 74px;">
<td style="width: 11%; height: 74px;">
<h1 id="布尔值">布尔值</h1>
</td>
<td style="width: 18%; height: 74px;">&nbsp;</td>
<td style="width: 10%; height: 74px;">&nbsp;
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> isDone: <span class="hljs-built_in">boolean</span> = <span class="hljs-literal">false</span>;</code></pre>
</td>
<td style="width: 43%; height: 74px;">&nbsp;</td>
<td style="width: 7%; height: 74px;">&nbsp;</td>
<td style="width: 0.541272%; height: 74px;">&nbsp;</td>
</tr>
<tr style="height: 74px;">
<td style="width: 11%; height: 74px;">
<h1 id="数字">数字</h1>
</td>
<td style="width: 18%; height: 74px;">&nbsp;</td>
<td style="width: 10%; height: 74px;">
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> decLiteral: <span class="hljs-built_in">number</span> = <span class="hljs-number">6</span>;</code></pre>
&nbsp;</td>
<td style="width: 43%; height: 74px;">&nbsp;</td>
<td style="width: 7%; height: 74px;">&nbsp;</td>
<td style="width: 0.541272%; height: 74px;">&nbsp;</td>
</tr>
<tr style="height: 74px;">
<td style="width: 11%; height: 74px;">
<h1 id="字符串">字符串</h1>
</td>
<td style="width: 18%; height: 74px;">&nbsp;</td>
<td style="width: 10%; height: 74px;">&nbsp;
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> name: <span class="hljs-built_in">string</span> = <span class="hljs-string">"bob"</span>;</code></pre>
</td>
<td style="width: 43%; height: 74px;">&nbsp;</td>
<td style="width: 7%; height: 74px;">&nbsp;</td>
<td style="width: 0.541272%; height: 74px;">&nbsp;</td>
</tr>
<tr style="height: 78px;">
<td style="width: 11%; height: 78px;">
<h1 id="数组">数组</h1>
</td>
<td style="width: 18%; height: 78px;">&nbsp;</td>
<td style="width: 10%; height: 78px;">
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> list: <span class="hljs-built_in">number</span>[] = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>];</code></pre>
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> list: <span class="hljs-built_in">Array</span>&lt;<span class="hljs-built_in">number</span>&gt; = [<span class="hljs-number">1</span>, <span class="hljs-number">2</span>, <span class="hljs-number">3</span>];</code></pre>
</td>
<td style="width: 43%; height: 78px;">&nbsp;</td>
<td style="width: 7%; height: 78px;">&nbsp;</td>
<td style="width: 0.541272%; height: 78px;">&nbsp;</td>
</tr>
<tr style="height: 126px;">
<td style="width: 11%; height: 126px;">
<h1 id="元组-tuple">元组 Tuple</h1>
</td>
<td style="width: 18%; height: 126px;">一个已知元素数量和类型的数组，各元素的类型不必相同。</td>
<td style="width: 10%; height: 126px;">
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> x: [<span class="hljs-built_in">string</span>, <span class="hljs-built_in">number</span>];</code></pre>
<pre><code class="lang-ts">x = [<span class="hljs-string">'hello'</span>, <span class="hljs-number">10</span>]; <span class="hljs-comment">// OK</span></code></pre>
&nbsp;</td>
<td style="width: 43%; height: 126px;">当访问一个越界的元素，会使用联合类型替代：</td>
<td style="width: 7%; height: 126px;">&nbsp;</td>
<td style="width: 0.541272%; height: 126px;">&nbsp;</td>
</tr>
<tr style="height: 82px;">
<td style="width: 11%; height: 82px;">
<h1 id="枚举">枚举</h1>
</td>
<td style="width: 18%; height: 82px;">&nbsp;</td>
<td style="width: 10%; height: 82px;">&nbsp;
<pre><code class="lang-ts"><span class="hljs-keyword">enum</span> Color {Red, Green, Blue}
<span class="hljs-keyword">let</span> c: Color = Color.Green;</code></pre>
</td>
<td style="width: 43%; height: 82px;">&nbsp;</td>
<td style="width: 7%; height: 82px;">&nbsp;</td>
<td style="width: 0.541272%; height: 82px;">&nbsp;</td>
</tr>
<tr style="height: 74px;">
<td style="width: 11%; height: 74px;">
<h1 id="任意值">任意值 any</h1>
</td>
<td style="width: 18%; height: 74px;">&nbsp;</td>
<td style="width: 10%; height: 74px;">&nbsp;
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> notSure: <span class="hljs-built_in">any</span> = <span class="hljs-number">4</span>;</code></pre>
</td>
<td style="width: 43%; height: 74px;">&nbsp;</td>
<td style="width: 7%; height: 74px;">&nbsp;</td>
<td style="width: 0.541272%; height: 74px;">&nbsp;</td>
</tr>
<tr style="height: 100px;">
<td style="width: 11%; height: 100px;">
<h1 id="空值">空值 void</h1>
</td>
<td style="width: 18%; height: 100px;">&nbsp;</td>
<td style="width: 10%; height: 100px;">&nbsp;
<pre><code class="lang-ts"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">warnUser</span>(): <span class="hljs-title">void</span> </span>{
    <span class="hljs-built_in">console</span>.log(<span class="hljs-string">"This is my warning message"</span>);
}</code></pre>
</td>
<td style="width: 43%; height: 100px;">&nbsp;</td>
<td style="width: 7%; height: 100px;">&nbsp;</td>
<td style="width: 0.541272%; height: 100px;">&nbsp;</td>
</tr>
<tr style="height: 31px;">
<td style="width: 11%; height: 31px;">
<h1 id="空值">Null 和 Undefined</h1>
</td>
<td style="width: 18%; height: 31px;">&nbsp;</td>
<td style="width: 10%; height: 31px;">
<pre><code class="lang-ts"><span class="hljs-keyword">let</span> u: <span class="hljs-literal">undefined</span> = <span class="hljs-literal">undefined</span>;
<span class="hljs-keyword">let</span> n: <span class="hljs-literal">null</span> = <span class="hljs-literal">null</span>;</code></pre>
&nbsp;</td>
<td style="width: 43%; height: 31px;">所有类型的子类型</td>
<td style="width: 7%; height: 31px;">&nbsp;</td>
<td style="width: 0.541272%; height: 31px;">&nbsp;</td>
</tr>
<tr style="height: 630px;">
<td style="width: 11%; height: 630px;">
<h1 id="空值">Never</h1>
</td>
<td style="width: 18%; height: 630px;">永不存在的值的类型</td>
<td style="width: 10%; height: 630px;">
<pre><code class="lang-ts"><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">error</span>(<span class="hljs-params">message: <span class="hljs-built_in">string</span></span>): <span class="hljs-title">never</span> </span>{
    <span class="hljs-keyword">throw</span> <span class="hljs-keyword">new</span> <span class="hljs-built_in">Error</span>(message);
}</code></pre>
&nbsp;</td>
<td style="width: 43%; height: 630px;">所有类型的子类型也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。</td>
<td style="width: 7%; height: 630px;">那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是never类型，当它们被永不为真的类型保护所约束时。</td>
<td style="width: 0.541272%; height: 630px;">&nbsp;</td>
</tr>
<tr style="height: 162px;">
<td style="width: 11%; height: 162px;">
<h1 id="空值">Object</h1>
</td>
<td style="width: 18%; height: 162px;">除number，string，boolean，symbol，null或undefined之外的类型</td>
<td style="width: 10%; height: 162px;">
<pre><code class="lang-ts"><span class="hljs-keyword">declare</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">create</span>(<span class="hljs-params">o: object | <span class="hljs-literal">null</span></span>): <span class="hljs-title">void</span></span>;
</code></pre>
&nbsp;</td>
<td style="width: 43%; height: 162px;">所有类型的子类型</td>
<td style="width: 7%; height: 162px;">&nbsp;</td>
<td style="width: 0.541272%; height: 162px;">&nbsp;</td>
</tr>
</tbody>
</table>

---



