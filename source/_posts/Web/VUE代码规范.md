---
title: VUE代码规范
categories:
  - Web
tags:
  - javascript
  - vuejs
abbrlink: c9ca51c5
date: 2019-07-22 14:21:08
---

为规范代码特写了本文，但实际由于历史包袱，还有很多jQuery时代的代码，加上人员技术水平不齐，执行起来非常有难度

## 1. 目的

1. 易于阅读和理解
2. 有助于debug
3. 方便接手的人
4. 不挖坑

## 2. 目录和文件规范

### 2.1 目录结构

```
src                                  源码目录
|-- api                              接口，统一管理
|-- assets                           静态资源，统一管理
|-- components                       公用组件，全局文件
|-- config                           配置信息
|-- filters                          过滤器，全局工具
|-- icons                            图标，全局资源
|-- layout                           页面模板
|-- libs                             公用工具库
|-- mock                             模拟接口，临时存放
|-- store                            vuex, 状态管理
|-- router                           路由，统一管理
|-- views                            视图目录
|   |-- account                      视图模块名
|   |-- |-- mygroup.vue              模块入口页面
```

<!-- more -->

### 2.2 vue文件结构

```js
<template>
  <div>
    <!-- html -->
  </div>
</template>

<script>
export default {
  components : {
  },
  data () {
    return {
    }
  },
  mounted() {
  }，
  methods: {
  }
}
</script>

<!--声明语言，并添加scoped-->
<style lang="less" scoped>
</style>
```

### 2.3 vue文件命名

文件名遵循驼峰标准，模块+功能，所以文件名至少包含2个单词，比如 `userList`、`memberAdd`


## 3. script规范

### 3.1 对象顺序

一般来说，data和methods是使用最多，data有助于理清页面相关绑定数据，而methods是代码最多的

推荐data放最前，methods放最后，这样有利于阅读和查找代码

其它对象没有特殊要求，我个人习惯这样排

```
  - data
  - props
  - components
  - computed
  - filter
  - watch
  - mounted
  - metods
```

### 3.2 指令规范

1. v-bind 和 v-on使用简写 :value 和 @click
2. v-for需加上key，并确保key是循环体内唯一的
3. 对于大段落html使用v-if和v-else的，需加上注释标明开始和结束
4. 无需刷新的数据用v-once
5. 非必要情况不使用v-html

### 3.3 emit事件规范

暴露给外部的回调事件，需符合html组件，使用简介明确的名称，比如 `click`、`onChange`、`onSuccess`

用户主动触发的事件，使用动作单词，如 `click`、`input`

非用户主动触发的事件，是用 on+事件名称，如 `onFail`、`onClose`

禁止使用复杂命名，比如 `goMemberDetail`、`submitComment`、`goCustomer`


### 3.4 props规范

禁止不定义参数类型，`props: ['status']`

除了类型外，建议带上注释和默认值
```js
// 是否显示[步骤1]
showStep: {
  type: Boolean,
  default: false
}
```


## 4. 注释规范

以下场景必须添加注释

1. 通用组件使用说明
2. 函数说明
3. 复杂业务逻辑说明
4. 存在已知问题的代码
5. 复杂 if 判断
6.

### 4.1 函数注释

```js
/**
  * 方法名称
  * @module 从属模块
  * @desc 描述
  * @author 作者
  * @date 2019-7-22 16:40:23
  * @param {Object} [title]    - 参数说明
  * @param {String} [columns] - 参数说明
  * @return {Object} 返回数据说明
  * @example 调用示例
  *  getById(xxx)
  **/
```

简单的函数，可以描述以下具体作用即可，推荐加上参数说明和参数类型

### 4.2 TODO注释

等待完善的代码，建议加上 TODO，附上未完成的功能说明
如
```js
// TODO 未加异常判断
```

另外还有 FIXME，在一些已知问题代码，并优先度较高的，可加此注释，每次发版本前搜一下有没遗忘


## 5. CSS规范

1. CSS能做的不用JS
2. 数值为0时不加单位，如`0px`应写为`0`
3. 组件内用scoped作用域
4.

待补充



