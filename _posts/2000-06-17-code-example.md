---
author: lexiao
comments: true
date: 2019-06-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React 之 code example
wordpress_id: 440
tags: recent fe
categories:

- react
- 前端
---

# code example



## event , class, callback

```js

class Toggle extends React.Component {  //////////////
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);     //////
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
        ////////////
      <button onClick={this.handleClick}>       
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>

      //  or 
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>


      // send value to event handler
      <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
      <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
    );
  }
}
```

---

## Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```js

class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```

---


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


## 列表 & Key, 渲染多个组件

[example](https://www.robinwieruch.de/react-list-component)

```js
function NumberList(props) {
  const numbers = props.numbers;    // 组件接收 numbers 数组作为参数
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

---

## Ref


```js

class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();     //////////////
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();          /////// current
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />    ///  ref=
    );
  }
}
```















