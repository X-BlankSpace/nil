---
title: Ubuntu下自定义cursor theme
date: 2017-08-13 17:20:08
toc: true
comments: true
tags: 
- Ubuntu
- cursor
categories: 
- Linux
---

找到喜欢的cursor theme似乎是一件艰难的事...不如自定义好了
##	首先准备一张无背景的图片
一般大小是32x32，暂且命名为arrow.png
##	确定鼠标点击位置的坐标
具体到像素，如（1,1）
## 	创建配置文件
1.暂且命名为arrow.cursor

2.输入<图片水平/垂直像素数> <坐标x> <坐标y> <图片文件名>，如 32 1 1 arrow.png

<!--more-->

##	生成X11指针
定位到当前文件夹，输入命令 
{% codeblock lang:bash %}
xcursorgen arrow.cursor arrow
{% endcodeblock %}
即可生成X11指针，即arrow文件
## 	替换当前的cursor
1.在/usr/share/icons/文件夹下找到当前cursor theme的文件夹

2.建议复制一份，重新命名，然后在新文件夹下替换。也可以直接替换

3.将主题文件夹中的arrow文件替换为刚才生成的arrow文件，重新选择主题即可
## 	附言
1.替换其他图样方法类似，不赘述

2.如果要替换的图样较多，建议使用GIMP，可以方便地查看X11指针文件，例如可以打开之前生成的arrow文件

3.建议使用Unity Tweak Tool，可以方便地进行各种设置

4.如果浏览器等应用中的cursor图样没有改变，重启即可。
