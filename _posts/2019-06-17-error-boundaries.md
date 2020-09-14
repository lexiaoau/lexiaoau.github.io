---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React高级 之 错误边界
wordpress_id: 440
categories:
- jsx
- react
---

# 错误边界

[Official Doc](https://zh-hans.reactjs.org/docs/error-boundaries.html)


部分 UI 的 JavaScript 错误不应该导致**整个应用崩溃**，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

错误边界是一种 React 组件，这种组件可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会**渲染出备用 UI，而不是渲染那些崩溃了的子组件树**。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。


如果一个 class 组件中定义了 static getDerivedStateFromError() 或 componentDidCatch() 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。当抛出错误后，请:

    - 使用 static getDerivedStateFromError() 渲染备用 UI ，
    - 使用 componentDidCatch() 打印错误信息。

```js

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

// >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }
  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< */

// >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
  componentDidCatch(error, info) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }
  // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< */

  render() {
      // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< */
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
      // <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< */
    }

    return this.props.children; 
  }
}
```

```js

<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

## 未捕获错误（Uncaught Errors）的新行为

### 自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载。


## 关于事件处理器

### 错误边界无法捕获事件处理器内部的错误。

React 不需要错误边界来从事件处理程序的错误中恢复。与 render 方法和生命周期方法不同，事件处理器不会在渲染期间触发。因此，如果它们抛出异常，React 仍然能够知道需要在屏幕上显示什么。

如果你需要在事件处理器内部捕获错误，使用普通的 JavaScript try / catch 语句：


*[HTML]: Hyper Text Markup Language

### [Custom containers](https://github.com/markdown-it/markdown-it-container)

::: warning
*here be dragons*
:::



