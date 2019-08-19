---
layout: post
title: 超过4g大文件无法复制到u盘
categories: Windows
description: NTFS
keywords: NTFS, FAT32
---

# 超过4g大文件无法复制到u盘

日常我们发现拷贝的文件到U盘时，提示无法文件过大（超过大于4G文件），无法完成复制。U盘FAT32格式不支持大于4G的单个文件存储，需要将FAT32格式转化成NTFS格式就好了。

## U盘格式

主要有三种:

● **FAT32**：

缺点：单个文件不能超过4GB，不支持512MB以下容量的U盘

备注：如果U盘容量达8GB以上，发现4GB文件拷不进去的话，可以考虑换用NTFS或ExFAT格式了

● **NTFS**：

优点：兼容性好，支持任意大小的U盘

缺点：会缩短闪存寿命

备注：U盘超便宜，使用很多年不会坏。

● **ExFAT**：

备注：专为闪存和U盘设计，空间浪费小，使用需要打过补丁的新系统。可以支持苹果和windows系统的对拷。

## 解决方法

### 一.格式化

1.首先，插入U盘，先把U盘里重要的数据备份起来，因为转化过程中需要格式化U盘。

![u盘1.jpeg](https://i.loli.net/2019/08/19/zD1tAxRjer6QFT8.jpg)

2.右击--格式化，在“文件系统”下拉列表中选择“NTFS”，然后点击“确定”进行格式化；

![u盘2.jpeg](https://i.loli.net/2019/08/19/uXK6bNQk4RW9ype.jpg)

最后进行复制，就可以复制4G以上大文件。

### 二.命令行

插入U盘后，使用组合键“Win+R”打开运行窗口，输入“CMD”后按回车；

![u盘3.jpeg](https://i.loli.net/2019/08/19/Xr51eLuRc6WAGD7.jpg)

在弹出的窗口中输入“convert G: /fs:ntfs”（G为U盘盘符）回车，等待执行命令，转化完成。如图所示：

![u盘4.jpeg](https://i.loli.net/2019/08/19/ri38BtV9NhSxwEo.jpg)