---
author: lexiao
comments: true
date: 2019-06-27 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React Hook 之 Hook 概览
wordpress_id: 440
categories:

- react
---

# Hook 概览

[Official Doc](https://zh-hans.reactjs.org/docs/hooks-overview.html)


## Hook 使用规则

- JavaScript 函数
- 只能在函数最外层调用 Hook。
    - **不要在循环、条件判断或者子函数中调用。**    
- 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中。）
    - 不要在普通的 JavaScript 函数中调用 Hook。


## 自定义 Hook

- Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。
- 如果函数的名字以 “use” 开头并调用其他 Hook，我们就说这是一个自定义 Hook。 useSomething 的命名约定



---

State Hook example:


```js

import React, { useState } from 'react';    ////////////

  function Example() {
    const [count, setCount] = useState(0);      /////////////////

    return (
      <div>
        <p>You clicked {count} times</p>    ///////////////
        <button onClick={() => setCount(count + 1)}>    //////////
         Click me
        </button>
      </div>
    );
  }
```

---

Effect Hook example:


```js

function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);        ////
  useEffect(() => {                         ///////
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);       ////
  useEffect(() => {                                     ////
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {                  /////
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  // ...
}
```
