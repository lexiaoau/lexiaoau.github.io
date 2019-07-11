---
author: lexiao
comments: true
date: 2019-07-10 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: Typescript 之 code example
wordpress_id: 440
tags: recent fe
categories:
- react
---

```typescript
for (let i: number = 0; i < 5; ++i) {
  let line: string = "";
  for(let j: number = 0; j <= i; ++j) {
    line += "*";
  }
  console.log(line);
}

let isDone: boolean = false;

// error TS2322: Type 'Boolean' is not assignableto type 'boolean'.
//  'boolean' is a primitive, but 'Boolean' is a wrapper object.Prefer using 'boolean' when possible.
// let createdByNewBoolean: boolean = new Boolean(1);

let createdByNewBoolean: Boolean = new Boolean(1);
let createdByBoolean: boolean = Boolean(1);
```

























