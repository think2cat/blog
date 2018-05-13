---
title: grunt入门笔记（1）
tags:
  - grunt
  - javascript
  - nodejs
id: 1803
categories:
  - Web
abbrlink: b722d741
date: 2017-05-15 00:14:47
---
![grunt.js](/images/2017/05/gruntjs.jpg)

#### 1. 安装

grunt依赖node.js，需要先安装node.js，官网[https://nodejs.org/](https://nodejs.org/)
安装后敲node -v会打印版本号，则安装成功
接着安装grunt， -g表示全局，不带则安装到当前目录
npm install -g grunt-cli

#### 2. 初始化package.json

package.json是node.js项目描述文件，在项目根目录下执行
```cmd
npm init
```
<!--more-->
根据提示输入即可，生成文件大概是这样子的
```json
{
	"name": "grunt-test",
	"version": "1.0.0",
	"description": "only for grunt test",
	"main": "index.js",
	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"repository": {
		"type": "git",
		"url": "git+https://github.com/think2cat/AdminLTE.git"
	},
	"keywords": [
		"grunt"
	],
	"author": "",
	"license": "MIT",
	"bugs": {
		"url": "https://github.com/think2cat/AdminLTE/issues"
	},
	"homepage": "https://github.com/think2cat/AdminLTE#readme"
}
```

#### 3. 安装grunt和常用插件
```cmd
npm install grunt --save-dev
```
安装后package.json多出一个字段
```json
"devDependencies": {
    "grunt": "^1.0.1"
}
```
这里devDependencies表示开发环境所依赖的库，如果是发布后需要的库则用 --save，对应package.json中的dependencies属性，如果只是安装库而不改动package.json则无须加参数
grunt常用插件
* 合并文件：[grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat)
* 语法检查：[grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint)
* CSS压缩： [grunt-contrib-cssmin](https://github.com/gruntjs/grunt-contrib-cssmin)
* Scss 编译：[grunt-contrib-sass](https://github.com/gruntjs/grunt-contrib-sass)
* 压缩文件：[grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify)
* 监听文件变动：[grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch)
* 建立本地服务器：[grunt-contrib-connect](https://github.com/gruntjs/grunt-contrib-connect)

#### 4. 配置grunt

grunt为js文件，遵循node.js模块语法
```js
module.exports = function(grunt) {
    //grunt code
};
```
grunt代码分为3部分，1）config配置，2）task任务，3）插件加载
config和task是卸载initConfig函数内，如下
```js
grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
    }
});
```
pkg从package.json读取配置，后面可以直接使用<%= pkg.name %>.来引用常量
还有另一种常见配置
```js
var config = {
	name: "test",
	author: "Gavin"
};
grunt.initConfig({
	pkg: config,
	build: {
		src: 'src/<%= pkg.name %>.js',
		dest: 'build/<%= pkg.name %>.min.js'
	}
});
```

#### 5. 配置task

grunt只是一个框架，实际干活的是众多插件，在安装完插件后，会在 node_modules 目录下生成对应插件目录，使用时在grunt.js里加载插件并编写相应task
比如常见的grunt-contrib-uglify
通过这样载入
```js
grunt.loadNpmTasks('grunt-contrib-uglify');
```
并对应到grunt.initConfig的 uglift 任务，如下
```js
uglify: {
	options: {
		mangle: true,
		preserveComments: 'some'
	},
	my_target: {
		files: {
			'dist/js/app.min.js': ['dist/js/app.js']
		}
	}
},
```

任务的名称uglify是跟插件名称对应的，不可改，完成上面配置后，再注册默认任务
```js
grunt.registerTask('default', ['uglify']);
```
此时执行grunt则会默认执行uglify任务，如果有多个任务，可以依次添加
```js
grunt.registerTask('lint', ['jshint', 'csslint', 'bootlint']);
```
执行时用命令
```cmd
grunt lint
```