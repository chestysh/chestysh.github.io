---
title: webpack
date: 2017-07-05 16:40:03
tags:
---

<!--more-->

### script 标签
 
```
 <script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```

这是最原始的 JavaScript 文件加载方式，如果把每一个文件看做是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在 window 对象中，不同模块的接口调用都是一个作用域中，一些复杂的框架，会使用命名空间的概念来组织这些模块的接口

这种原始的加载方式暴露了一些显而易见的弊端：

* 全局作用域下容易造成变量冲突
* 文件只能按照 script 的书写顺序进行加载
* 开发人员必须主观解决模块和代码库的依赖关系
* 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

### CommonJS

> 这种加载方式跟微信小程序应该是一样。

服务器端的 Node.js 遵循 [CommonJS](http://wiki.commonjs.org/wiki/CommonJS)规范，该规范的核心思想是允许模块通过 require 方法来同步加载所要依赖的其他模块，然后通过 exports 或 module.exports 来导出需要暴露的接口。
```
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```
优点：
* 服务器端模块便于重用
* [NPM](https://www.npmjs.com/) 中已经有将近20万个可以使用模块包
* 简单并容易使用

缺点：
* 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
* 不能非阻塞的并行加载多个模块

### ES6 模块

>这种模块的加载方式没有尝试过

ECMAScript6 标准增加了 JavaScript 语言层面的模块体系定义。ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。

```
import "jquery";
export function doStuff() {}
module "localModule" {}
```
优点：
* 容易进行静态分析
* 面向未来的 ECMAScript 标准

缺点:
* 原生浏览器端还没有实现该标准
* 全新的命令字，新版的 Node.js才支持

### Webpack 的特点
>针对webpack以下所说的特点，有一些目前我还是无法理解，需要更加深入的学习
#### 代码拆分
Webpack 有两种组织模块依赖的方式，同步和异步。异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。

#### Loader
Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

#### 智能解析
Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 ``equire("./templates/" + name + ".jade")``

#### 插件系统
Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求。

#### 快速运行
Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。

## 目录结构

>这个是github上的一个开源项目代码目录，也是我主要学习的代码,网络请求呢主要使用axios

[项目地址](https://github.com/PanJiaChen/vue-element-admin/wiki)
```shell
├── build                      // 构建相关
├── config                     // 配置相关
├── src                        // 源代码
│   ├── api                    // 所有请求
│   ├── assets                 // 主题 字体等静态资源
│   ├── components             // 全局公用组件
│   ├── directive              // 全局指令
│   ├── filtres                // 全局filter
│   ├── mock                   // mock数据
│   ├── router                 // 路由
│   ├── store                  // 全局store管理
│   ├── styles                 // 全局样式
│   ├── utils                  // 全局公用方法
│   ├── view                   // view
│   ├── App.vue                // 入口页面
│   └── main.js                // 入口 加载组件 初始化等
├── static                     // 第三方不打包资源
│   ├── jquery
│   └── Tinymce                // 富文本
├── .babelrc                   // babel-loader 配置
├── eslintrc.js                // eslint 配置项
├── .gitignore                 // git 忽略项
├── favicon.ico                // favicon图标
├── index.html                 // html模板
└── package.json               // package.json

```

## 目录结构介绍 
[项目地址](https://github.com/lin-xin/manage-system.git)
```
	|-- build                            // webpack配置文件
	|-- config                           // 项目打包路径
	|-- src                              // 源码目录
	|   |-- components                   // 组件
	|       |-- common                   // 公共组件
	|           |-- Header.vue           // 公共头部
	|           |-- Home.vue           	 // 公共路由入口
	|           |-- Sidebar.vue          // 公共左边栏
	|		|-- page                   	 // 主要路由页面
	|           |-- BaseCharts.vue       // 基础图表
	|           |-- BaseForm.vue         // 基础表单
	|           |-- BaseTable.vue        // 基础表格
	|           |-- Login.vue          	 // 登录
	|           |-- Markdown.vue         // markdown组件
	|           |-- MixCharts.vue        // 混合图表
	|           |-- Readme.vue           // 自述组件
	|           |-- Upload.vue           // 图片上传
	|           |-- VueEditor.vue        // 富文本编辑器
	|           |-- VueTable.vue         // vue表格组件
	|   |-- App.vue                      // 页面入口文件
	|   |-- main.js                      // 程序入口文件，加载各种公共组件
	|-- .babelrc                         // ES6语法编译配置
	|-- .editorconfig                    // 代码编写规格
	|-- .gitignore                       // 忽略的文件
	|-- index.html                       // 入口html文件
	|-- package.json                     // 项目及工具的依赖配置文件
	|-- README.md                        // 说明
```