---
author: lexiao
comments: true
date: 2020-10-16 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: JavaScript , Typescript 等易混淆代码比较
wordpress_id: 440
tags: recent fe
categories:
- javascript
- 前端
---

# JavaScript 函数

****   普通 function 

```js

function myFunc(theObject) {
  theObject.make = "Toyota";
}
myFunc(mycar);



```

****   匿名函数

```js

const square = function(number) { return number * number; };
var x = square(4); // x gets the value 16

```

# JavaScript 函数参数

****   默认参数

```js

// 参数提供默认值的函数
function log(x, y = 'World') {
  console.log(x, y);
}

// 只有当函数foo的参数是一个对象时，变量x和y才会通过解构赋值生成。
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined

// 如果没有提供参数，函数foo的参数默认为一个空对象。
function foo({x, y = 5} = {}) {
  console.log(x, y);
}

foo() // undefined 5

```

****   rest 参数

```js

// rest 参数是对应一个数组，包含了所有剩余参数的数组
function add(...values) {
  let sum = 0;
  for (var val of values) {sum += val;}
  return sum;
}

add(2, 5, 3) // 10

```



# JavaScript 箭头函数

****   使用 function 关键字

```js

// 单参数、单返回值语句箭头函数
var a3 = a.map( s => s.length );
// 2参数、单返回值语句箭头函数
var a3 = values.reduce((a, b) => a + b, 0);

// 0参数、无返回值箭头函数
  setInterval(() => {
    this.age++; // 这里的`this`正确地指向person对象
  }, 1000);


// 单参数、无返回值**多**语句箭头函数
$("#confetti-btn").click(event => {
  playTrumpet();
  fireConfettiCannon();
});  

// 箭头函数与变量解构结合使用，相当于参数是object，object里有first和last两个attribute
const full = ({ first, last }) => first + ' ' + last;


//////////////////  rest 参数与箭头函数结合

const numbers = (...nums) => nums;
numbers(1, 2, 3, 4, 5)
// [1,2,3,4,5]

const headAndTail = (head, ...tail) => [head, tail];
headAndTail(1, 2, 3, 4, 5)
// [1,[2,3,4,5]]


```

- 箭头函数内花括号的判断（object？function？）
    - 在 ES6中，规定： 箭头（=>）之后的花括号（{）,判断为函数体的边界符号；而并非对象（object）的边界符号。


```js

//  XXXXXX    错误，花括号被判断为函数体边界，而空函数返回的是 undefined
var chewToys = puppies.map(puppy => {});   // BUG!
//  ✔️✅✔✅  ， 花括号被（）包围起来，而不是紧跟着箭头，所以被判断为object边界符号，返回的是一个空object
var chewToys = puppies.map(puppy => ({})); // ok

```

# JavaScript Symbol 易混淆代码

****   用于object内部 attribute 的标识符 

```js

let s = Symbol();

let obj = {
  [s]: function (arg) { ... }
//   [s](arg) { ... }      /////////  这2行代码是等价的，但是这种写法很难看懂
};

obj[s](123);



```

- Symbol要用方括号包围













