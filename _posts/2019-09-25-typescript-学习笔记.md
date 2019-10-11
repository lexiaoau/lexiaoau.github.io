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


# typescript 数据类型

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


# 类

```js

class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");

```

## 继承

```js

class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);

```

- 子类使用 super() 调用父类的构造函数
- 方法可以被覆盖
- 调用哪个类的方法是根据 runtime instance type

## 公共，私有与受保护的修饰符

- 默认为public
- 构造函数也可以被标记成protected。 这意味着这个类不能在包含它的类外被实例化，但是能被继承。

## readonly修饰符

只读属性必须在声明时或构造函数里被初始化。

```js

class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.

```

## 参数属性

参数属性通过给构造函数参数添加一个访问限定符来声明。 使用private限定一个参数属性会声明并初始化一个私有成员；对于public和protected来说也是一样。

```js

class Animal {
    constructor(private name: string) { }               // use private
    move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

```

## 存取器

```js

let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {             ////////////  use get
        return this._fullName;
    }

    set fullName(newName: string) {  ////////////  use set
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";                    ////  to set
if (employee.fullName) {                        //////  to get
    alert(employee.fullName);
}


```

## 静态属性

```js

class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);              /////   Grid.origin.x
        return ...;
    }
    constructor (public scale: number) { }
}


```

## 抽象类

```js

abstract class Department {

    constructor(public name: string) {    }

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void; // 必须在派生类中实现
}

```

- 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 
- 抽象方法必须包含abstract关键字并且可以包含访问修饰符。

## 类是一种类型，可以用在任何使用 interface 的地方

---
---
---

# 函数

## 为函数添加类型的方式

```js

function add(x: number, y: number): number {
    return x + y;
}

```

```js

let myAdd = function(x: number, y: number): number { return x + y; };

```

```js

let myAdd: (x:number, y:number) => number =             //// => 后面是函数返回值类型
    function(x: number, y: number): number { return x + y; };

```

```js



```

```js



```

## 特殊参数

### 可选参数

- 在参数名旁使用?实现可选参数的功能。
- 可选参数必须跟在必须参数后面。
- 

### 有默认初始化值的参数

- 在所有必须参数后面的带默认初始化的参数都是可选的，
- 带默认值的参数不需要放在必须参数的后面。可以传 undefined

```js

function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // okay and returns "Bob Adams"
let result4 = buildName(undefined, "Adams");     // okay and returns "Will Adams"

```

### 剩余参数

- 想同时操作多个参数，或者并不知道会有多少参数传递进来。
- 剩余参数个数  >= 0

```js

function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");

let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;

```


## 泛型

```js

////
////    example         1
////
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}

////
////    example         2
////
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}

////
////    example         3
////
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}



```


### 泛型类型

以下例子创建了一个接口，一个泛型函数，并且把函数赋值給了一个变量。
接口是描述了函数的一些特点（例如参数类型和个数，函数返回值）; 在这个描述中使用泛型来指代具体类型。

```js

interface GenericIdentityFn {
    <T>(arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn = identity;

```

```js

interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;

```






















