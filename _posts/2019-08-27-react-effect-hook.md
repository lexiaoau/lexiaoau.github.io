---
author: lexiao
comments: true
date: 2019-08-27 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React Hook 之 Effect
wordpress_id: 440
categories:
    
    - react
---

# 使用 Effect Hook

[Official Doc](https://zh-hans.reactjs.org/docs/hooks-effect.html)

[Hook Example](https://codesandbox.io/react-hooks)


## Effect Hook 使用规则

-   其名称以 “useEffect” 开头
-   如果函数内部返回一个函数，那么该返回函数会在清除 Effect 时调用（类似于 componentWillUnmount ）。

## 如何正确使用多个 Effect Hook

- 就像你可以使用多个 state 的 Hook 一样，也可以使用多个 effect 。
- 其作用是允许我们按照代码的用途分离他们
- 默认清除行为是，在调用一个新的 effect 之前对前一个 effect 进行清理。
    - 如果某些特定值在两次重渲染之间没有发生变化，你可以通知 React 跳过对 effect 的调用，只要传递数组作为 useEffect 的第二个可选参数即可。
    - 如果数组中有多个元素，即使只有一个元素发生变化，React 也会执行 effect。
    - 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。

例子： 跳过对 effect 的调用，只要传递数组作为 useEffect 的第二个可选参数

```js
useEffect(() => {
    document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```

```js
useEffect(() => {
    function handleStatusChange(status) {
        setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
        ChatAPI.unsubscribeFromFriendStatus(
            props.friend.id,
            handleStatusChange
        );
    };
}, [props.friend.id]); // 仅在 props.friend.id 发生变化时，重新订阅
```

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

---
