---
author: lexiao
comments: true
date: 2019-06-06 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: React 之 React 哲学
wordpress_id: 440
categories:
- jsx
- react
- 前端
---

# React 哲学

[Official Doc](https://zh-hans.reactjs.org/docs/thinking-in-react.html)

## 从设计稿开始

## 1. 将设计好的 UI 划分为组件层级

* 在设计稿上用方框圈出每一个组件（包括它们的子组件），并且以合适的名称命名。
* 单一功能原则来判定组件的范围。也就是说，一个组件原则上只能负责一个功能。

## 2. 用 React 创建一个静态版本

* 先用已有的数据模型渲染一个不包含交互功能的 UI。
    - 最好将渲染 UI (大量代码，不需要交互细节) 和添加交互(大量细节，不需要太多代码) 这两个过程分开。
* 通过 props 传入所需的数据。props 是父组件向子组件传递数据的方式
    - 完全不应该使用 state 构建静态版本。**state 代表了随时间会产生变化的数据** ,  应当仅在实现交互时使用。 

## 3.   确定 UI state 的最小（且完整）表示  

* 通过问自己以下三个问题，你可以逐个检查相应数据是否属于 state

    1. 该数据是否是由父组件通过 props 传递而来的？      不 -> state。
    2. 该数据是否随时间的推移而保持不变？               不 -> state。
    3. 能否根据其他 state 或 props 计算出该数据的值？   不 -> state。

        - 综上所述，属于 state 的有：
            - 用户输入的搜索词
            - 复选框是否选中的值

## 4.   确定 state 放置的位置

* 需要确定哪个组件能够改变这些 state，或者说拥有这些 state。


对于应用中的每一个 state:

- 找到根据这个 state 进行渲染的所有组件。
- 找到他们的共同所有者（common owner）组件（在组件层级上高于所有需要该 state 的组件）。
- 该共同所有者组件或者比它层级更高的组件应该拥有该 state。
- 如果你找不到一个合适的位置来存放该 state，就可以直接创建一个新的组件来存放该 state，并将这一新组件置于高于共同所有者组件层级的位置。            

## 5. 添加反向数据流

将尝试让数据反向传递：处于较低层级的表单组件更新较高层级的 state。
