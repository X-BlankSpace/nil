---
title: Node.js实现从图虫网下载图片集
date: 2017-05-20 09:59:58
toc: true
tags: 
- Node.js 
- RegExp
categories: 
- Node.js
---
最近图虫网改版了，半年前写的代码已经不再适用，于是对代码进行整理，重写部分代码，并做了适当的优化。代码已push至<a href="https://github.com/X-BlankSpace/DownloadPicturesFromTuchong.com">GitHub</a>。
项目主要实现了从图虫网下载图片到本地。其中

## 下载方式分为两种
1. 指定标签（如风光、街拍等）、下载的图片集数量(num)以及排序（热门或最新），下载该标签下序号为1-num的图片集，图片保存在 PicturesFromTuchong\"相应标签" 文件夹下。
2. 指定图片集地址，下载该图片集，图片保存在 PicturesFromTuchong\singleSet 文件夹下。

同时，每个图片集保存在一个文件夹中，文件夹里包含一个描述图片集信息的README.txt文档。

<!--more-->

## 项目主要由五个文件组成
> 1. index.js，用于启动项目
2. sever.js，用于创建服务器
3. router.js，用于路由
4. requestHandlers.js，用于处理请求
5. index.html，主界面

## 项目的核心部分
项目的核心部分是requestHandlers.js中的upload函数，用于处理提交的表单信息。upload函数主要由四个函数组成
> 1. Queue，用于生成队列，控制request并发数不大于5
2. getPostUrl，获取每个post（图集）的url
3. getDownloadLinksAndDownload，获取每张图片的下载地址
4. downLoadSingleImage，下载单张图片

## 经验与感悟
### 获取postUrl
图虫网改版之前很容易通过page和tag等来获取postUrl，改版之后藏得有点点深，不过页面用的是动态加载的瀑布流，所以拉滚动条就会加载新的post。于是我在页面中的ul.pagelist-wrapper设置了breakpoint，对其子树进行监听，在//static.tuchong.net/js/pc/common/lib_38e9ec6.js下发现了
``` 
e = Object {url: "/rest/tags/%E5%B0%91%E5%A5%B3/posts?page=7&count=20&order=weekly", type: "GET"...}
```
在浏览器中键入上面这个url（加上协议和图虫的域名），进行一些尝试并分析服务器返回的信息就可以根据需求获取postUrl了。

### Node.js的request并发数不能超过5
使用队列来控制并发数，activeNum为当前并发数，在入队出队时分别判断activeNum是否小于5，若是，则取出队列的下一个request执行。

### 以用户设置的title命名文件夹极容易出错
虽然对文件夹中不能出现的字符进行了过滤，但仍然容易出错。比如虽然 '.' 是允许出现的，但不能出现在文件名的最后一位，这样会导致新建的文件夹不能用普通方法访问和删除。此外，空格以及回车符也很容易导致错误。所以，为了兼顾可读性和程序正确性，虽然把上述错误几乎都处理了一遍，但我最后选择了过滤掉title中除字母和数字外的所有字符，毕竟它们携带的含义相对较少而且容易出错，整个程序崩溃可是很让人痛心的。

title为空也是时有的事，这时我用author+postid替换title。还有一个容易出错的地方是文件名有可能重复，这时，我会在后面加一小串随机数，并且确认没有同名文件夹了才新建文件夹。不过，最好只对每个图片集所在的文件夹进行同名文件夹检测和处理，否则用起来会很揪心。

### 给每个request 3次机会
由于网络等原因可能会导致request失败，这时我会再重新request两次，这样一般都能下载成功。
