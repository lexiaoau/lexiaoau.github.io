---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React高级 之 Refs 转发
wordpress_id: 440
categories:
- jsx
- react
- 前端
---

# Refs 转发

[Official Doc](https://zh-hans.reactjs.org/docs/forwarding-refs.html)

- 用途:
    - 将 ref 自动地通过组件传递到其一子组件的技巧
    - 这个技巧对高阶组件（也被称为 HOC）特别有用。
        - 如果你对 HOC 添加 ref，该 ref 将引用最外层的容器组件，而不是被包裹的组件。



- 注意
    - 对于大多数应用中的组件来说，这通常不是必需的。
    - 在组件库中使用 forwardRef 时，应当将其视为一个破坏性更改，并发布库的一个新的主版本。因为库可能会有明显不同的行为,并且这样可能会导致依赖旧行为的应用和其他库崩溃。
 



Example:

```js

function logProps(Component) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      console.log('new props:', this.props);
    }

    render() {
      const {forwardedRef, ...rest} = this.props;

      // 将自定义的 prop 属性 “forwardedRef” 定义为 ref
      return <Component ref={forwardedRef} {...rest} />;
    }
  }

  // 注意 React.forwardRef 回调的第二个参数 “ref”。
  // 我们可以将其作为常规 prop 属性传递给 LogProps，例如 “forwardedRef”
  // 然后它就可以被挂载到被 LogPros 包裹的子组件上。
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
```

---



```js

const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

以下是对上述示例发生情况的逐步解释：

1. 我们通过调用 React.createRef 创建了一个 React ref 并将其赋值给 ref 变量。
2. 我们通过指定 ref 为 JSX 属性，将其向下传递给 \<FancyButton ref={ref}\>。
3. React 传递 ref 给 fowardRef 内函数 (props, ref) => ...，作为其第二个参数。
4. 我们向下转发该 ref 参数到 \<button ref={ref}\>，将其指定为 JSX 属性。
当 ref 挂载完成，ref.current 将指向 \<button\> DOM 节点。


