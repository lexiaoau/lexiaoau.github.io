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
<tr>
<td style="width: 99px;">
<h1 id="布尔值">布尔值</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">

```js
let isDone: boolean = false;
```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="数字">数字</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">

```js
let decLiteral: number = 6;
```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="字符串">字符串</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">


```js
let name: string = "bob";
```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="数组">数组</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">

```js
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="元组-tuple">元组 Tuple</h1>
</td>
<td style="width: 99px;">一个已知元素数量和类型的数组，各元素的类型不必相同。</td>
<td style="width: 100px;">

```js
let x: [string, number];
x = ['hello', 10]; // OK

```

</td>
<td style="width: 100px;">当访问一个越界的元素，会使用联合类型替代：</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="枚举">枚举</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">

```js
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
let colorName: string = Color[2];

```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="任意值">任意值</h1>
</td>
<td style="width: 99px;">&nbsp;</td>
<td style="width: 100px;">

```js

let notSure: any = 4;

```

</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>
<tr>
<td style="width: 99px;">
<h1 id="空值">空值</h1>
</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 150px;">

```js

function warnUser(): void {
    console.log("This is my warning message");
}

```

</td>

<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>

<tr>
<td style="width: 99px;">
<h1 id="空值">Null 和 Undefined</h1>
</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 150px;">

```js

let u: undefined = undefined;
let n: null = null;

```

</td>

<td style="width: 100px;">所有类型的子类型</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>

<tr>
<td style="width: 99px;">
<h1 id="空值">Never</h1>
</td>
<td style="width: 100px;">永不存在的值的类型</td>
<td style="width: 150px;">

```js

function error(message: string): never {
    throw new Error(message);
}

```

</td>

<td style="width: 100px;">所有类型的子类型也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。</td>
<td style="width: 100px;">那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是never类型，当它们被永不为真的类型保护所约束时。</td>
<td style="width: 100px;">&nbsp;</td>
</tr>

<tr>
<td style="width: 99px;">
<h1 id="空值">Object</h1>
</td>
<td style="width: 100px;">除number，string，boolean，symbol，null或undefined之外的类型</td>
<td style="width: 150px;">

```js

declare function create(o: object | null): void;

```

</td>

<td style="width: 100px;">所有类型的子类型</td>
<td style="width: 100px;">&nbsp;</td>
<td style="width: 100px;">&nbsp;</td>
</tr>

</tbody>
</table>

---



