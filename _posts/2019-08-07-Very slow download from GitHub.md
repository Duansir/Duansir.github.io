---
layout: post
title: Github下载速度慢
categories: Github
description: Very slow download from GitHub
keywords: Github 
---

# ●解决Github国内下载速度慢问题

因为大家都知道的原因，在国内从github上面下载代码的速度峰值通常都是20kB/s。这种速度对于那些小项目还好，而对于大一些的并且带有很多子模块的项目来讲就跟耽误时间。而常见的的方法无非就是修改HOST或者挂VPN，实际用起来并不稳定。

### 修改HOST文件

#### 第一步，打开本机上的Hosts文件 

首先，什么是Hosts文件？

> 在互联网协议中，host表示能够同其他机器互相访问的本地计算机。一台本地机有唯一标志代码，同网络掩码一起组成IP地址，如果通过点到点协议通过ISP访问互联网，那么在连接期间将会拥有唯一的IP地址，这段时间内，你的主机就是一个host。
>
> 在这种情况下，host表示一个网络节点。host是根据TCP/IP for Windows 的标准来工作的，它的作用是包含IP地址和Host name(主机名)的映射关系，是一个映射IP地址和Host name(主机名)的规定，规定要求每段只能包括一个映射关系，IP地址要放在每段的最前面，空格后再写上映射的Host name主机名　。对于这段的映射说明用“#”分割后用文字说明。

##### ~Windows

Hosts文件的路径是：

**C:\Windows\System32\drivers\etc**

由于文件没有后缀名，可以利用鼠标右键点击，选择用记事本打开，如下图。

 

![img](https://upload-images.jianshu.io/upload_images/10482796-b3d057d8d3b69f5f.png)

 

##### ~Mac 

终端内输入：

**sudo vim /etc/hosts**

打开之后，我们就要向里面追加信息了。

#### 第二步，追加域名的IP地址

我们可以利用https://www.ipaddress.com/ 来获得以下两个GitHub域名的IP地址：

(1) github.com

(2) github.global.ssl.fastly.net

打开网页后，利用输入框内分别查询两个域名：

 

![img](https://upload-images.jianshu.io/upload_images/10482796-d4b8d060d057b6f1.png)

 

先试一下github.com：

 

![img](https://upload-images.jianshu.io/upload_images/10482796-c26549b216011c9a.png)

 

在标注的IP地址中，任选一个记录下来。

再来是github.global.ssl.fastly.net：

 

![img](https://upload-images.jianshu.io/upload_images/10482796-2748b78e2e38b87d.png)

 

将以上两段IP写入Hosts文件中：

 

![img](https://upload-images.jianshu.io/upload_images/10482796-c789141b563632cf.png)

保存。

#### 第三步，刷新DNS缓存

在终端或CMD中，执行以下命令：

 **ipconfig /flushdns**

收工。

### VPN翻墙

[自力更生](https://github.com/bannedbook/fanqiang/wiki)

### 新方法

这里提供一种新的方法，下载速度可以达到 1~2MB/s

#### 1、利用开源中国提供的代码仓库

标题已经说的很清楚了，我想对于经常使用git的人来讲，很可能已经知道了。对于新手刚接触git的人来讲，可能你只知道github。
实际上，国内也有很多代码仓库提供方，国外也不只github。只不过国内也是刚刚开始，关注的人不多。
开源中国提供的代码仓库提供了一个功能，就是它可以将github账号中的代码 clone 到开源中国的账户中去。这个代码仓库叫做 **码云** ，没错就是码云😂。
要求你有一个**github**账户，一个码云**gitee**账户。
步骤很简单

将github上面你想要搞下来的项目首先 frok 到你自己的github的账户中去。耗时:一瞬间
登录gitee，没有的自行注册。网页中有添加项目的按钮，一个加号。点击加号，下拉列表里面有 迁移github项目 的选项，点开后按照提示关联自己的github账号，之后选择你要迁移的项目，按提示操作。耗时:不到三分钟。
按照 clone github项目方法， clone 迁移到gitee账户中的项目。区别是 clone 链接换成了目标项目在gitee中的链接。通常下载速度是以MB/s为单位的。
按照上面的方法，基本上不再需要整夜挂机 clone 代码了。

#### 2、提高下载子模块的速度

有的项目里用到了第三方代码仓库，但是在你使用 clone 指令的时候这些子模块 submodule 并不会自动下载，因为他们在另外的地址中存放。你需要 clone 完目标项目后，执行

**git submodule update --init --recursive**

才会将目标项目所需要的依赖子模块下载下来。github项目中所用到的子模块依然是放在了github上。这就很悲剧了，这意味着你在执行上面指令后，依然需要面对上面的20KB/s的速度。虽然此时并不会显示出来，然而等待依然很久。

我们同样使用上面加速 clone 的思路。

从下载的项目中找到其使用的 submodule 的链接是哪里。
打开上一步中的链接，将使用的目标子模块的代码同样 frok 到自己的github账户中，之后同样的方法迁移到gitee中去。有多个子模块就多重复几次操作，同样的套路。
将原项目使用的 submodule 模块的链接地址修改为子模块迁移到gitee中后的地址。

这时再去执行git submodule update --init --recursive 。




原文链接：https://www.jianshu.com/p/0493dcc15d6f

原文链接：https://blog.csdn.net/kcx64/article/details/83866633