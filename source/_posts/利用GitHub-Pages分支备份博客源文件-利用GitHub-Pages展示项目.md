---
title: 利用GitHub Pages分支备份博客源代码 & 利用GitHub Pages展示项目
date: 2017-03-20 10:40:43
tags: GitHub
categories: 
- GitHub
---
使用hexo，如果「换了电脑」或者「博客源代码丢失」了怎么方便地更新博客？
其实hexo生成的文件里有.gitignore，本意应该也是想我们把这些文件push到GitHub上。利用GitHub Pages分支备份博客源代码，原理很简单，就是在博客对应的Repository里添加一个分支backup，把源文件push到这个分支上，把hexo生成的静态网页文件deploy在master分支上。<!--more-->
# 利用GitHub Pages分支备份博客源代码
## 步骤如下
<ul>
	<li>在博客对应的Repository里找到Branches按钮，在搜索框里输入backup，创建一个新的分支</li>
    <li>在setting--Branches--Default branch中将backup设置为默认的分支，这样git push时就会push到这个分支上</li>
	<li>在本地任意新建一个文件夹，clone博客对应的Repository到这里，复制.git文件夹到你的本地博客的hexo文件夹下，并把hexo文件夹下的_config.yml里的deploy--branch参数修改为master</li>
	<li>在git客户端中定位到hexo文件夹，使用命令`git checkout -b backup`新建本地分支backup,然后`git push origin backup`即可</li>
</ul>
## 日常更新博客
<ul>
	<li>对博客进行更新</li>
	<li>依次执行`git add .` 、 `git commit -m "some message"` 、 `git push origin backup`推送源文件</li>
	<li>依次执行`hexo g` 、 `hexo d` 生成、部署博客</li>
</ul>
# 利用GitHub Pages展示项目
<br/>找到要展示的项目所在的Repository，在setting--GitHub Pages里修改source中的branch为项目所在的分支（master或其他分支），并保存设置。选择一个Theme或者不选==
接着上方出现了一个网址，即可通过https://YourGitHubName.github.io/RepositoryName 访问你的项目。
一个Repository里可以存放多个项目，访问时在上面的网址后添加相对路径即可。若要访问单个文件，直接加上该文件的相对路径，也可以将HTML文件重命名为index.html，再添加其所在项目文件夹的相对路径即可。

> 参考链接： https://www.zhihu.com/question/21193762/answer/79109280



