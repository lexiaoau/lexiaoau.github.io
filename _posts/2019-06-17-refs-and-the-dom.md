---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React高级 之 refs-and-the-dom
wordpress_id: 440
categories:

- react
---

# REFS AND DOM

[Official Doc](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html)

- 用途:
    - 通过创建一个特殊引用，以控制一个DOM元素（或者React组件）的变化。
    - 下面是几个适合使用 refs 的情况：
      - 管理焦点，文本选择或媒体播放。
      - 触发强制动画。
      - 集成第三方 DOM 库。
    - ref 的值根据节点的类型而有所不同：
      - 当 ref 属性用于 HTML 元素时，构造函数中使用 React.createRef() 创建的 ref 接收**底层 DOM 元素**作为其 current 属性。
      - 当 ref 属性用于自定义 class 组件时，ref 对象接收**自定义 class的挂载实例**作为其 current 属性。
    - 挂载生命周期
      - React 会在组件挂载时给 current 属性传入 DOM 元素，
      - 并在组件卸载时传入 null 值。
      - ref 会在 componentDidMount 或 componentDidUpdate 生命周期钩子触发前更新。


- 注意
    - 避免使用 refs 来做任何可以通过声明式实现来完成的事情。
    - HOC 应该透传与自身无关的 props。
    - 不要在 render 方法中使用 HOC
    - 务必复制静态方法
    - Refs 不会被传递
    - 不能在函数组件上使用 ref 属性，因为它们没有实例 (See: ref 对象接收**自定义 class的挂载实例**作为其 current 属性)

- 将 DOM Refs 暴露给父组件
  - 在极少数情况下，你可能希望在父组件中引用子节点的 DOM 节点。通常不建议这样做，
  - 如果使用 16.3+ 版本的 React, 这种情况下推荐使用 ref 转发。
  - 如果使用 16.2- 版本的 React, 可以使用这个替代方案 [dom_ref_forwarding_alternatives_before_16.3](https://gist.github.com/gaearon/1a018a023347fe1c2476073330cc5509)

- 回调 Refs
  - 更精细地控制何时 refs 被设置和解除
  - 传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问
  - React 将在组件挂载时，会调用 ref 回调函数并传入 DOM 元素，当卸载时调用它并传入 null。
  - 在 componentDidMount 或 componentDidUpdate 触发前，React 会保证 refs 一定是最新的。
  - 可以在组件间传递回调形式的 refs，就像你可以传递通过 React.createRef() 创建的对象 refs 一样。




Example:

```js

class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // 创建一个 ref 来存储 textInput 的 DOM 元素    /////////// 创建
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // 直接使用原生 API 使 text 输入框获得焦点
    // 注意：我们通过 "current" 来访问 DOM 节点
    this.textInput.current.focus();
  }

  render() {
    // 告诉 React 我们想把 <input> ref 关联到
    // 构造器里创建的 `textInput` 上
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />

        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

2nd:

```js

render() {
  // 过滤掉非此 HOC 额外的 props，且不要进行透传
  const { extraProp, ...passThroughProps } = this.props;

  // 将 props 注入到被包装的组件中。
  // 通常为 state 的值或者实例方法。
  const injectedProp = someStateOrInstanceMethod;

  // 将 props 传递给被包装组件
  return (
    <WrappedComponent
      injectedProp={injectedProp}
      {...passThroughProps}
    />
  );
}
```

---

回调 Refs

```js

class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }

  render() {
    // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```


```js

function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

// Parent 把它的 refs 回调函数当作 inputRef props 传递给了 CustomTextInput，而且 CustomTextInput 把相同的函数作为特殊的 ref 属性传递给了 <input>。
// 结果是，在 Parent 中的 this.inputElement 会被设置为与 CustomTextInput 中的 input 元素相对应的 DOM 节点。
class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```


