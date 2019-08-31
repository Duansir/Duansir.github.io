---
layout: post
title: 换行符引发的问题
categories: Ubuntu
description: Git上传时报错LF will be replaced by CRLF
keywords: Ubuntu LF
---

## 不同系统下的换行符

​       首先问题出在不同操作系统所使用的换行符是不一样的，下面罗列一下三大主流操作系统的换行符：

1、**Uinx/Linux**采用换行符**LF**表示下一行（LF：LineFeed，中文意思是换行）；

2、**Dos**和**Windows**采用回车+换行**CRLF**表示下一行（CRLF：CarriageReturn LineFeed，中文意思是回车换行）；

3、**Mac OS**采用回车**CR**表示下一行（CR：CarriageReturn，中文意思是回车）。



## 出现问题

​      1、在windows和ubuntu双系统下，编辑md文件后，Git上传时报错：“warning: LF will be replaced by CRLF in ……”

​      2、github博客首页目录显示全部md文章内容，而不只是标题，显得很乱。



## 解决方法

​       1、找到win项目的.git目录,修改config文件，在[core]配置项最后添加下面一句话，就不会报错了。
**autocrlf = false**

![config.png](https://i.loli.net/2019/08/31/qnujQbErPO9FZ32.png)

​                   git bash查看配置

![LF.png](https://i.loli.net/2019/08/31/y5kmNFRYLp8tM34.png)



​        2、md文件选择，Typora->编辑->换行符->Unix换行符(**LF**)