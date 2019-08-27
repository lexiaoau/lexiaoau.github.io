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

# 使用 State Hook

[Official Doc](https://zh-hans.reactjs.org/docs/hooks-state.html)

## Hook 通用规则

-   不要在循环，条件或嵌套函数中调用 Hook， 确保总是在你的 React 函数的最顶层调用他们。
-   只在 React 函数组件中调用 Hook

## State Hook 使用规则

-   其名称以 “use” 开头
-   函数内部可以调用其他的 Hook。
-   useState 前的等号之前的部分，是包含两个元素的数组，第一个元素是 state 名字（如下例子中的 isOnline），第二个元素是用以更改 state 值的函数名（如下例子中的 setIsOnline）。
-   useState 的函数参数可以是数值，对象，数组等等。该参数指定了 "该 state" 的类型和初始值（如下例子中的 null ）。

例子

```js
import React, { useState, useEffect } from "react";

function useFriendStatus(friendID) {
    //// start with "use", param
    const [isOnline, setIsOnline] = useState(null); ////

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

## 自定义 Hook

-   Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。
-   如果函数的名字以 “use” 开头并调用其他 Hook，我们就说这是一个自定义 Hook。 useSomething 的命名约定

---

提取自定义 Hook example:

code 1:

```js
import React, { useState, useEffect } from "react";

function useFriendStatus(friendID) {
    //// start with "use", param
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

code 2:

```js
function FriendStatus(props) {
    const isOnline = useFriendStatus(props.friend.id);

    if (isOnline === null) {
        return "Loading...";
    }
    return isOnline ? "Online" : "Offline";
}
```

code 3:

```js
function FriendListItem(props) {
    const isOnline = useFriendStatus(props.friend.id);

    return (
        <li style={{ color: isOnline ? "green" : "black" }}>
            {props.friend.name}
        </li>
    );
}
```

---
