---
layout: post
title: Ubuntu+Windows双系统安装
categories: Ubuntu
description: Windows + Ubuntu 16.04 双系统安装详细教程
keywords: Ubuntu
---

# Windows + Ubuntu 16.04 双系统安装详细教程

## 国内源

[中科大源 ](http://mirrors.ustc.edu.cn/ubuntu-releases/ )

[阿里云开源镜像站](http://mirrors.aliyun.com/ubuntu-releases/ )

[兰州大学开源镜像站](http://mirror.lzu.edu.cn/ubuntu-releases/)

[北京理工大学开源](http://mirror.bit.edu.cn/ubuntu-releases/)

[浙江大学](http://mirrors.zju.edu.cn/ubuntu-releases/) 

## 安装教程

**安装主要分为以下几步：**

- 一． 下载Ubuntu 16.04镜像软件； 
- 二． 制作U盘启动盘使用ultraISO； 
- 三． 安装Ubuntu系统； 
- 四． 用EasyBCD 创建系统启动引导； （根据个人情况，选择性的安装）
- 五． 开启系统；

### 一、下载ubuntu16.04

直接到国内源免费下载 

根据自己计算机的配置信息下载（本人下载的是的64位的）

### 二 、制作U盘启动器 

 百度下载ultraISO软件安装并打开 

  1、![ultraISO1.jpg](https://i.loli.net/2019/08/19/RlcL1ZjQGUHzu7h.jpg)

2、

![ultraISO2.jpg](https://i.loli.net/2019/08/19/Hh6VxgB8LATbIor.jpg)

3、

![ultraISO3.jpg](https://i.loli.net/2019/08/19/6dToyZGMhLOXf7q.jpg)

4、开始写入—直到完成，大概五分的样子 。

### 三、安装Ubuntu系统 

**1、要在Windows下新划出一个大于20G的硬盘空间**

（建议分非系统盘，本人划分了120G） 

在win7系统下–》计算机–》右键–》管理–》磁盘管理—–》压缩卷。

> 右键点击我的电脑，然后选择“管理”。如图示，然后选择磁盘管理。

![分区1.jpg](https://i.loli.net/2019/08/19/DUFrbX1uaW263l8.png)

> 进入磁盘管理之后，我们可以看到我的分区情况。然后选择你需要进行分区的磁盘，点击右键，然后选择“压缩卷”，如图示。这个操作比较的简单

![分区2.jpg](https://i.loli.net/2019/08/19/xWnCUjhzw6EOa9e.png)

> 然后系统会自动的查询压缩空间，这个时间就需要看电脑的配置。

![分区3.jpg](https://i.loli.net/2019/08/19/9Aa3erQuS5OyI2z.png)

> 然后我们选择选择好需要压缩空间的大小。点击压缩即可。

![分区4.jpg](https://i.loli.net/2019/08/19/HxS7DCVFI5frJGv.png)

> 等一会，空间就压缩好了。如图示会出现一个可用的空间。这就是我们刚才压缩出来的空间。如图所示。

![分区5.jpg](https://i.loli.net/2019/08/19/fWsbrd6ih2J4POu.png)

**2.在电脑上插入制作好的U盘启动盘，重启电脑，F2–>boot界面，选择通过USB启动。**

（不同主板进入boot，按键有区别） 

**3.进入ubuntu安装菜单，选择 “安装Ubuntu”。**

（语言选择汉语吧！当然你的英文可以了，English无所谓了） 

![Ubuntu1.png](https://i.loli.net/2019/08/19/FNWtX9g2UdDZBbc.png)

**4.准备安装: 接下来会进入“准备安装Ubuntu”界面：这里勾选“为图形或无线硬件….”,然后点击“继续”。**

（这里会检测是否已经连网，没网的话，那个 "安装Ubuntu时下载更新" 的是不能选的）:

![Ubuntu3.png](https://i.loli.net/2019/08/19/9zFXS568VN4jGMD.png)

**5.在安装类型界然后选择最后一项“其他选项”，以为这样可以自己手动分区，点击继续。** 

![Ubuntu2.png](https://i.loli.net/2019/08/19/loRz43x5u8NfPcW.png)

**6.现在我们看到的是硬盘的分区情况，找到前边有“空闲”二字，我们要做到就是，把空闲的空间给ubuntu划分分区。 点击“空闲”的分区，选择下边的“添加”，弹出窗口如下，上边填写分区空间大小，下边填写要挂载的分区，然后确定。** 

逻辑分区，300M，起始，Ext4日志文件系统，**/boot**；（引导分区200M足够）

 ![boot.png](https://i.loli.net/2019/08/19/bTP8pjHc2OeQYom.png)

逻辑分区，5G，起始，交换空间，无挂载点；（交换分区swap，一般不大于物理内存） 

![swap.png](https://i.loli.net/2019/08/19/9aUmhVYqpAFiO7T.png)

逻辑分区，30G，起始，Ext4日志文件系统，**/**；（系统分区”/”或称作”/root”装系统和软件，15G以上足够） 

![根目录.png](https://i.loli.net/2019/08/19/rUGvu9Q62tLRMDY.png)

逻辑分区，剩余空间数70G，起始，Ext4日志文件系统，**/home**；（home分区存放个人文档） 

![home.png](https://i.loli.net/2019/08/19/7elQqzN9VpvyTr6.png)

**7.分区设置好后，查看/boot分区的编号，然后在下边的“安装启动引导区的设备”下拉框中选择/boot分区的编号，点击安装。**一直到你安装成功，当然中途需要你设置一个用户名和密码的（这个就不说了吧） 

![引导.png](https://i.loli.net/2019/08/19/b2nlWpyQqgrXh16.png)

**8.检查分区:**

![检查分区.png](https://i.loli.net/2019/08/19/MGyp6ns8mob7zTt.png)

**9.安装完成后需要重新启动**（重启之后会发现，没有Ubuntu的选择项，依旧直接进入Windows，别急，往下看） 

### 四. 用EasyBCD 创建启动系统。

1.下载EasyBCD，此软件用于在启动电脑的时候选择要进入的系统（自行百度搜索安装）

2.打开easyBCD，选择add new entry, 选择linux/BSD, name这一行随便填写，只是系统名词，写ubuntu吧，Device这一行选择刚刚我们创建的300MB的那个”/boot“分区,前边有linux标记的。（其他的不要动） 

![easybcd1.png](https://i.loli.net/2019/08/19/n3agP5hz49LTyMf.png)
![easybcd2.png](https://i.loli.net/2019/08/19/p5mW2VA1UOlqjHJ.png)

### 五. 开启系统 

做完这些重启系统后，系统会将win7系统和ubuntu 16.04系统都列出来，你可以选择系统进入了。 

这样启动的好处（windows 不会受到Ubuntu的影响） 

如果说没有最后这一步，没有任何问题，但是你要是启动windows7把Ubuntu系统的分区删除，那么就启动不了系统了 

（如果你真的遇到这样的问题了，不要着急，直接用老毛桃U盘或者大白菜制作好的U盘启动，直接启动引导修复就OK了）

1.选择Ubuntu-16.04 

![启动1.jpg](https://i.loli.net/2019/08/19/y8RsJvES9LOCnTY.jpg)

 2.选择Ubuntu或者等待几秒自动进入
![启动2.jpg](https://i.loli.net/2019/08/19/LvdREboOuyW68nV.jpg)


 3.进入系统主界面

 ![启动3.jpg](https://i.loli.net/2019/08/19/wT79tILzoGVSaqx.jpg)



到这里就结束了……恭喜你了！

[参考]: https://blog.csdn.net/flyyufenfei/article/details/79187656

