---
title: Axios笔记1
tags:
  - axios
  - javascript
categories:
  - Web
abbrlink: e16c0619
date: 2018-08-24 13:02:03
---

Axios 是基于 promise 的http库，可在nodejs服务端或客户端浏览器运行
其中浏览器使用XMLHttpRequests对象进行请求
[Github地址](https://github.com/axios/axios/)

## 特点
1. 支持服务端和客户端
2. 基于Promise
3. 支持请求和响应拦截
4. 支持JSON格式转换
5. 支持取消请求

## 安装
```sh
npm install axios
```
或者
```sh
bower install axios
```
或者直接引入
```html
<script src="//raw.githubusercontent.com/axios/axios/9005a54a8b42be41ca49a31dcfda915d1a91c388/dist/axios.min.js"></script>
```

## 使用

### get
```js
axios.get('/info')
    .then(response => {
        console.log(response);
    })
    .catch(error => {
        console.log(error);
});
```
如果需要传参数，可以直接加在 url ，也可以用params
```js
axios.get('/info',{
        params: { id: 123, time: 12345}
    })
    .then(response => {
        console.log(response);
    })
    .catch(error => {
        console.log(error);
});
```

### post
```js
axios.post('/info',{
        id: 123, 
        name: 'Gavin'
    })
    .then(response => {
        console.log(response);
    })
    .catch(error => {
        console.log(error);
});
```
### request
get和post都是简化的请求，同时axios提供类似了jQuery.ajax的api，可以直接配config
上面2个改下如下
```js
axios.request({
    url: '/info',
    method: 'get',
    params: { id: 123, time: 12345}
    }.then(response => {
        console.log(response);
    }).catch(error => {
        console.log(error);
});

axios.request({
    url: '/info',
    method: 'post',
    data: {
        id: 123, 
        name: 'Gavin'
    }
    }.then(response => {
        console.log(response);
    }).catch(error => {
        console.log(error);
});
```

### delete
这个主要在RESTful规范API会用到
```js
axios.delete('/user/123')
    .then(response => {
        console.log(response);
    })
    .catch(error => {
        console.log(error);
});
```

### put
同delete方法，主要用于修改接口
```js
axios.put('/user/123', jsonObj)
    .then(response => {
        console.log(response);
    })
    .catch(error => {
        console.log(error);
});
```

### head
head用法同get，但是只返回header信息，不会有response body
可以节省很多带宽，主要用途
1. 测试连接可用性
2. 检验页面有没改动
3. 藏在header的数据检验
```js
axios.head("/info", { params: {"id": 123}})
    .then(
        response => console.info("headers:", response.headers)
    )
```

### all
同Promise的all，可以传入请求数组，当请求全部完成时执行回调
```js
axios.all([
  axios.get('//google.com'),
  axios.get('//163.com'),
  axios.get('//qq.com')
  ]).then(
      res => {
        console.log(res)
      }
  );
```
`all` 传入数组，回调返回的 `response` 其实也是数组
![image](/images/2018/08/axios1.png)

### spread
这个方法用来展开多个response返回值，我自己没实际用过，谷歌了也没发现具体用途，不知道跟数组是什么优势，有知道的麻烦说一下
```js
axios.all([
  axios.get('//google.com'),
  axios.get('//163.com'),
  axios.get('//qq.com')
  ]).then(
    axios.spread((res1,res2,res3) => {
      console.log(res1)
      console.log(res2)
      console.log(res3)
    })
  );
```
