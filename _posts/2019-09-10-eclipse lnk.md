---
layout: post
title: linux下创建eclipse快捷方式
categories: Ubuntu
description: 创建eclipse的启动快捷方式，方便使用 
keywords: Ubuntu eclipse
---

# linux下创建eclipse快捷方式

### 创建桌面快捷方式

1、cd进入/usr/share/applications文件夹.该文件夹就相当于Windows上的快捷方式。可以用 ls 查看一下都是以.desktop 结尾的文件。

![2019-10-08 17-35-30.png](https://i.loli.net/2019/10/08/nazmV3dRyhlNv52.png)

2、在此文件夹创建一个eclipse的快捷方式。sudo vim eclipse.desktop

3、输入i，添加如下内容：

```c
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse
Comment=Eclipse IDE
Exec=/usr/eclipse/eclipse     #eclipse存放路径
Icon=/usr/eclipse/icon.xpm  #eclipse存放路径
Terminal=false
Type=Application
Categories=GNOME;Application;Development;
StartupNotify=true
```

![001.png](https://i.loli.net/2019/10/08/ALw7UQHoOtaVWy1.png)

4、我们可以看到在usr/share/applacations/文件夹下有了eclipse的快捷方式

5、通过命令chmod  u+x  eclipse.desktop来改变eclipse.desktop的权限

### 使用软连接

```shell
sudo ln -s ~/usr/eclipse/eclipse /bin #创建一个连接在/bin中，即可在命令行中输入eclipse快速启动，具体路径以个人路径
```



# Ubuntu 18.04 截图工具推荐

1、安装命令：sudo apt-get install flameshot

2、设置>设备>键盘，设置一个自定义快捷键（拉到最下面）

​      命令填写：flameshot gui

​      快捷键设为f12



# Ubuntu更改国内源

![ubuntu源更改.png](https://i.loli.net/2019/10/08/rFW1zumiRf4pIoh.png)