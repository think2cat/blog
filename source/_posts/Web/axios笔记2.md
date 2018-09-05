---
title: Axios笔记2
tags:
  - axios
  - javascript
  - http
categories:
  - Web
abbrlink: 786557a3
date: 2018-08-31 17:11:44
---

## 实例化
上一篇说了基本用法，实际使用中并不会直接调用，而是实例化一个对象
并配置好基本参数，然后所有请求走实例化对象
```js
const service = axios.create({
  baseURL: '/api1.0/'
  timeout: 5000 // request timeout
})

function list(query) {
  return service({
    url: 'getList',
    method: 'post',
    data: query
  })
}
// 调用示例
list(query).then()
```
配置参数较多，参照 [Axios官网](https://github.com/axios/axios)

## 拦截器
axios支持拦截器，可以在请求和返回时进行拦截处理
<!--more-->
```js
axios.interceptors.request.use(config => config, error => error);
axios.interceptors.response.use(response => response, error => error);
```
拦截器同样是符合Promise标准
一般用的较多的是在请求时加入 `token` ，返回数据时进行数据过滤和权限判断

```js
service.interceptors.request.use(config => {
  // 让每个请求携带token
  config.headers['token'] = 'xxxx'
  return config
}, error => {
  console.log(error) // for debug
  Promise.reject(error)
})

service.interceptors.response.use(
  response => response,
  error => {
    console.log('request.err' + error)// for debug
    if (error.response && error.response.status === 401) {
      // session失效
      // 登录超时，请重新登录
      // do something
    }
    return Promise.reject(error)
  })
```

## 错误处理
Axios默认把 `200 <= status < 300` 归为 resolve，其它则为 reject
可以修改参数中 `validateStatus` 来进行修改
```js
validateStatus: function (status) {
  return status >= 200 && status < 300; // default
},
```
reject 时返回error对象，可以从 error.response 获取http对象
![error.response](/images/2018/08/axios2.png)

如果是本地发生的异常，还没到请求阶段，则没有 response 对象

## 取消请求
调用定时器 `setTimeout()` 会返回一个整形token，然后用 `clearTimeout()` 传入返回的token即可清除定时器
axios 的取消也是一样道理，只是不是请求时返回token，而是把token在请求时
```js
const source = axios.CancelToken.source()
console.log('cancelToken', source)
```
初始化一个 `CancelToken` 对象
![axios.CancelToken](/images/2018/08/axios3.png)
里面有个 `cancel()` 方法和名为 `token` 的 Promise对象

```js
// 请求时传入 cancelToken
request({
    url: 'list',
    method: 'post',
    data: query,
    cancelToken: source.token
}).catch(e => {
    console.log(e.message)
})
// 取消请求
source.cancel('some request is cancel')
```
此时 `catch` 会打印 `some request is cancel`







