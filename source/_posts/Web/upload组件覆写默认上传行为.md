---
title: 'element-ui upload组件覆写默认上传行为'
tags:
  - javascript
  - axios
categories:
  - Web
abbrlink: a91bebb6
date: 2018-09-20 11:15:35
---

element-ui 自带了上传组件，支持多文件，支持文件列表显示，支持定义各种钩子，非常方便

```html
<el-upload
    ref="upload"
    :show-file-list="false"
    :action="uploadUrl"
    :before-upload="handleBeforeUpload"
    :on-progress="handleUploading"
    :on-success="handleUpload"
    :on-error="handleUploadError"
    accept=".apk"
    :auto-upload="true">
    <el-button slot="trigger" :loading="uploadLoading" size="mini" type="primary">上传</el-button>
    <span v-html="tips"></span>
</el-upload>
```

同时也提供了 `http-request` 参数来自定义上传行为
<!-- more -->
```html
<el-upload
    ref="upload"
    :show-file-list="false"
    action=""
    :http-request="upload"
    :before-upload="handleBeforeUpload"
    :on-progress="handleUploading"
    accept=".apk"
    :auto-upload="true">
</el-upload>
```

`action` 虽然不再生效，但删掉这个参数会提示 `Missing required prop: "action"`，所以这里留空处理

覆写默认上传行为后，钩子除了 `before-upload` 其它都不起作用了，但依然可以在 html 保留指定，后面调用 `http-request` 指定的方法时会把整个文件对象作为参数传入

![upload](/images/2018/09/upload1.png)

传入 `upload()` 的参数，为upload组件对象，除了文件对象本身，还有3个钩子

```js
upload(fileObj) {
  const formData = new FormData()
  formData.append('file', fileObj.file)
  formData.append('type', fileObj.file.type)
  return axios.request({
    url: 'api/upload',
    method: 'post',
    data: formData
  }).then(this.handleUpload, this.handleUploadError)
}
```
调用axios执行上传，把 file对象塞到 `data` 即可

axios自带上传时钩子 `onUploadProgress`，如果需要显示上传进度，可把 upload组件的 `onProgress` 传给axios，修改如下

```js
upload(fileObj) {
  const formData = new FormData()
  formData.append('file', fileObj.file)
  formData.append('type', fileObj.file.type)
  return axios.request({
    url: 'api/upload',
    method: 'post',
    data: formData,
    onUploadProgress: progressEvent => {
       const complete = (progressEvent.loaded / progressEvent.total * 100 | 0)
       fileObj.onProgress({ percent: complete })
    },
  }).then(this.handleUpload, this.handleUploadError)
}
```

`onUploadProgress` 传入是 event 事件，如下图

![onUploadProgress](/images/2018/09/upload2.png)

因为 upload组件 `onProgress` 钩子有个 `percent` 百分比属性，所以直接计算后传回

上传完成回调，组件默认是返回 response 响应内容，而覆写后是返回 axios 对象，axios使用可以看 {% post_link 'Web/axios笔记1'%} 和 {% post_link 'Web/axios笔记2'%}

最后提一下，一般接口会配置共用config，并设置超时时间，而在上传大文件的时候很容易发生超时，则需要在 `request` 时加入 `timeout: undefined` 来防止