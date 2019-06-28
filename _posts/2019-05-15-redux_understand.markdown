---
author: lexiao
comments: true
date: 2019-05-15 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=441
published: true
slug: null
title: redux 理解
wordpress_id: 441
categories:
- react
- 前端
---

---

## Reducer example

注意:

- combineReducers的参数对象中，
    - 冒号（：）之前的, 会成为 state 中的一个属性。
    - 冒号（：）之后的, 是reducer函数名字。
    - [API Doc](https://redux.js.org/api/combinereducers)
- 单个reducer函数的规则：
    - 第一个参数，不管实参是什么名字，其形参是在combineReducers中其对应的state名，例如，对于selectedSongReducer（）函数，其state是selectedSong 。该参数可以指定默认值。
    - **永远**不可以返回 undefined。
    - 如果传入的state是undefined，函数应该返回state的正常初始值。（可以使用es6的默认参数值，例如selectedSong ）。
    - 当reducer函数遇到无法判定的action时，应该返回原始state。
    - 第二个参数是action，触发这次state变化的action。
    

```js

import { combineReducers } from 'redux';

const songsReducer = () => {
  return [
    { title: 'No Scrubs', duration: '4:05' },
    ......
    { title: 'I Want it That Way', duration: '1:45' }
  ];
};

const selectedSongReducer = (selectedSong = null, action) => {
  if (action.type === 'SONG_SELECTED') {
    return action.payload;
  }

  return selectedSong;
};

export default combineReducers({
  songs: songsReducer,
  selectedSong: selectedSongReducer
}); 

```
---

## Action example

注意:

- 必须定义type，其它属性可以自定义。
- 参数是由触发变动的函数传递给action。
- action creator是返回一个函数，函数里面返回具体action。



```js

// Action creator
export const selectSong = asong => {
  console.log(asong)
    // Return an action
    return {
      type: 'SONG_SELECTED',
      payload: asong
    };
  };
```

---

## CreateStore example

注意:

- 将combineReducers的返回结果与store绑定，实际上也是reducer里的state绑定到store中。
- 第二个参数是state的初始值，可以传空值。
- 第三个参数是中间件



```js

ReactDOM.render(
  <Provider store={createStore(reducers, {}, applyMiddleware(thunk, createLogger))}>
    <App />
  </Provider>,
  document.querySelector('#root')
);
```

---

## 需要使用state的组件 example
[API](https://react-redux.js.org/api/connect)

注意:

- 使用react-redux的connect（）函数，将自己和state绑定。
- connect（）函数解释。
    * 第一个参数, mapStateToProps, 将state中的属性和当前组件的props绑定到一起，以便以props的方式来处理state.
    * 第二个参数, mapDispatchToProps , 接受dispatch函数作为参数，把state改动源绑定到对应的action中.也可以把参数传给action。


API prototypes
```js

function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)
```


```js

import { connect } from 'react-redux';
import { selectSonga } from '../actions';

class SongList extends Component {  
  renderList() {
    return this.props.songs.map(song => {
      return (
        <div className="item" key={song.title}>
            <button
              onClick={() => this.props.selectSonga(song)}>
          <div className="content">{song.title}</div>
}

const mapDispatchToProps = (dispatch) => {
  return {
    onNumberAdd: (value) => dispatch(addNumber(value)),     onPinClear: () => dispatch(clearPin()),
    onLockChange: () => dispatch(changeLockStatus())
  }
}

const mapStateToProps = state => {
  return { songs: state.songs };
};

export default connect(
  mapStateToProps,
  { selectSonga }
)(SongList);
```

