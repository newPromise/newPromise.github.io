---
title: Vue 组件之间的传值
date: 2017-09-10 23:09:48
tags: 组件通信
categories: Vue
---

在 `vue` 中，进行组件通信有下面几种形式:

### 父子组件通信  

在 `vue` 中, 实现父子组件通信的方法主要是 :  props down,  emit up

使用 props 实现父组件向子组件传递信息, 在父组件上的子组件模板上绑定需要进行传递的数据

```
父组件中：
<child  :propsData = 'localData'></child>
子组件中：
<template>
</template>
<script>
  export default {
    name: 'child',
    // 子组件中通过使用 props 声明需要进行接受的数据
    props: ['propsData'],
    data () {
      return {
        localData: ''
      }
    }
  }
</script>
```

对于实现子组件向父组件的传值操作，使用 `emit()` 操作实现:



### 兄弟组件通信  

### Vuex

使用 `vuex` 实现多个组件之间状态的共享

![](Vue-组件之间的传值/vuex.png)

上图是使用 vuex 的工作流程，下面是我对于 `vuex` 的一些个人理解

`vuex` 主要是有三个部分组成: 

`actions` : 用来执行提交 mutations 操作  

`mutations` : 用来对于 `state` 中存储的数据进行改变操作  

`state` : 用来存储一些数据， 这些数据对于各个组件之间是可以共享的  

总的来说： 要执行一次进行改变 `state` 中状态的数据，要进行以下操作

`state` 中保存了要进行改变的数据状态  

`mutations` 中定义了要进行改变数据状态的操作，这是函数

我们通过 `actions`  中的函数对于在`mutations` 中定义的函数进行提交, 相当于执行函数，从而实现对于`state` 中保存数据的改变

 