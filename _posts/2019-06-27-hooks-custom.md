---
author: lexiao
comments: true
date: 2019-06-27 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React Hook 之 自定义 Hook
wordpress_id: 440
categories:
- jsx
- react
- 前端
---

# 自定义 Hook

[Official Doc](https://zh-hans.reactjs.org/docs/hooks-custom.html)


## 自定义 Hook 使用规则

- 自定义 Hook 是一个函数
- 其名称以 “use” 开头   
- 函数内部可以调用其他的 Hook。 


## 自定义 Hook

- Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。
- 如果函数的名字以 “use” 开头并调用其他 Hook，我们就说这是一个自定义 Hook。 useSomething 的命名约定



---

提取自定义 Hook example:


```js

import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {         //// start with "use", param
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

---

Use 1st example:


```js

function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```
---

Use 2nd example:


```js

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```
