---
title: "Postman 高级用法"
date: 2018-09-09 20:32:00
categories: tools
---

Postman，一款功能强大的HTTP调试软件(以前只是谷歌浏览器的插件，现在已经独立成软件)，最近接触到它的一些高级用法，才发现它原来并没有我想象中那么简单，还有很多的高级用法。其主要有如下功能:

- HTTP/API调试
- 测试API接口
- 生成API接口文档
- 监控API接口
- mock接口数据

下面列举一些常用的功能使用方法。

<!--more-->

## 主要功能

### 接口直接转换为代码

![接口转换为代码](https://haofly.net/uploads/postman_0.png)

点击上图中的`Code`，可以选择导出成HTTP、C(LibCurl)、cURL、C#(RestSharp)、Go、Java、JavaScript、NodeJS、Objective-C、Ocaml、PHP、Python、Ruby、Shell、Swift等多种语言的代码实现。

### 环境变量

点击第一张图片中`No Environment`右边的`设置按钮`可以添加环境变量。同一个接口在不同的环境下，可以设置不同的变量值，比如域名和IP，这个功能能让我们一键切换所需的环境变量。设置环境变量界面如下:

![](https://haofly.net/uploads/postman_1.png)



### 自动进行认证

`Postman`自带了多种认证方式，可以让你在请求前自动去进行认证。

![](https://haofly.net/uploads/postman_6.png)

### 请求示例

点击第一张图片里面的`examples(0)`可以添加请求示例，在这里可以添加请求的请求值与返回值的样例。可以给同一个接口添加多个示例。

### 添加cookie

点击第一张图片里面的`cookies`可以设置指定域名的cookies，一般是从网页上面直接拿下来的，例如如果该接口需要用户登录，但是又不想在`postman`里面写一遍用户登录接口，那么可以在网页上面直接将cookie复制下来即可。

### API Collections备注

在左侧的collections上面点击小箭头即可进入API collections的详情页面，在这里可以直接修改API集合的`Documentation`文档：

![](https://haofly.net/uploads/postman_2.png)

### 测试

基于js的测试工具，在`Tests`标签页预置了多个测试用的代码片段`SNIPPETS`

![](https://haofly.net/uploads/postman_7.png)

#### 测试整个Collections

在`Collections`的小箭头上面的`Run`能运行整个`Collections`中所有的测试，在这里可以选择运行测试需要的环境变量等：

![](https://haofly.net/uploads/postman_8.png)

![](https://haofly.net/uploads/postman_9.png)



## 奇淫技巧

### 直接粘贴json格式的headers头

需要注意，复制的时候前后一定不能有多余的空格，例如，复制`{"a": "b", "b": 2333}`，那么在`Headers`标签中的第一个key处直接`Ctrl + V`粘贴即可直接格式化

![](https://haofly.net/uploads/postman_3.png)

### 直接URLEncode和URLDecode

![](https://haofly.net/uploads/postman_4.png)

### 在path中定义变量

用冒号表示，下面的`Params`会自动添加

![](https://haofly.net/uploads/postman_5.png)