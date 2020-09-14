---
author: lexiao
comments: true
date: 2019-06-06 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React 之 组合 vs 继承
wordpress_id: 440
categories:

- react
---

# 组合 vs 继承

[Official Doc](https://zh-hans.reactjs.org/docs/composition-vs-inheritance.html)

## 特殊的 children prop

* JSX 标签中的所有内容都会作为一个 children prop 传递

```js
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}             {/*  use props.children */}
    </div>
  );
}
```

---

```js

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">  //// one child
        Welcome
      </h1>
      <p className="Dialog-message">     //// another child
        Thank you for visiting our spacecraft!    
      </p>
    </FancyBorder>
  );
}
```

---

```js

function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}        {/*  use left */}
      </div>
      <div className="SplitPane-right">
        {props.right}       {/*  use right */}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={        {/*  use left */}
        <Contacts />
      }
      right={       {/*  use right */}
        <Chat />
      } />
  );
}
```

---






