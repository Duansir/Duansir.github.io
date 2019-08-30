---
layout: post
title: Linux软件安装
categories: Ubuntu
description: 常用Linux软件安装
keywords: Ubuntu software
---



# Typora for Linux

```markdown
# or run:# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update

# install typora
sudo apt-get install typora
```

[typora](https://www.typora.io/#linux)

# thefuck 

```markdown
# install thefuck
sudo apt update
sudo apt install python3-dev python3-pip python3-setuptools
sudo pip3 install thefuck

# Updating
pip3 install thefuck --upgrade
```

[thefuck](https://github.com/nvbn/thefuck)

# **shadowsocks-qt5**     

```markdown
# 下载最新的版本是后缀为.AppImage的文件
https://github.com/shadowsocks/shadowsocks-qt5/releases

# 在下载文件夹给权限直接运行
 chmod a+x Shadowsocks-Qt5-3.0.1-x86_64.AppImage
 ./Shadowsocks-Qt5-3.0.1-x86_64.AppImage
 
 # Shadowsocks 设置方法 (Linux)
 https://github.com/Shadowsocks-Wiki/shadowsocks/blob/master/6-linux-setup-guide-cn.md
 
 # Chrome + Proxy SwitchyOmega 设置
 https://github.com/Shadowsocks-Wiki/shadowsocks/blob/master/7-2-chrome-setup-guide-cn.md
 
 # 自启动
 dash->启动应用程序->添加启动程序
```



# Electronic-Wechat

```markdown
# 安装Snap基本环境
sudo apt install snapd snappy
# 安装electronic-wechat
sudo snap install electronic-wechat
# 卸载electronic-wechat
sudo snap remove electronic-wechat
```



# Shutter

```markdown
# 安装截图工具 
sudo apt-get install shutter
# 卸载 
sudo apt-get remove shutter
sudo apt-get autoremove shutter
```



# 主题美化

```markdown
# 下载桌面外观管理工具
sudo apt-get install unity-tweak-tool
# 安装Flatabulous主题
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install flatabulous-theme
# 然后安装该主题配套的图标：
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons
# 配置主题
打开unity-tweak-tool，点击主题，修改为Flatabulous;点击图标，修改为Ultra-flat
```



# Sougou输入法

```markdown
# 下载安装包
http://pinyin.sogou.com/linux/
# 安装
sudo dpkg -i sogoupinyin_2.2.0.0082_amd64.deb
```



# Beyond Compare 4

```markdown
# 官网下载
http://www.scootersoftware.com/download.php
# 安装
sudo dpkg -i bcompare-4.2.10.23938_amd64.deb
# 破解
cd /usr/lib/beyondcompare/
sudo sed -i "s/keexjEP3t4Mue23hrnuPtY4TdcsqNiJL-5174TsUdLmJSIXKfG2NGPwBL6vnRPddT7tH29qpkneX63DO9ECSPE9rzY1zhThHERg8lHM9IBFT+rVuiY823aQJuqzxCKIE1bcDqM4wgW01FH6oCBP1G4ub01xmb4BGSUG6ZrjxWHJyNLyIlGvOhoY2HAYzEtzYGwxFZn2JZ66o4RONkXjX0DF9EzsdUef3UAS+JQ+fCYReLawdjEe6tXCv88GKaaPKWxCeaUL9PejICQgRQOLGOZtZQkLgAelrOtehxz5ANOOqCaJgy2mJLQVLM5SJ9Dli909c5ybvEhVmIC0dc9dWH+/N9KmiLVlKMU7RJqnE+WXEEPI1SgglmfmLc1yVH7dqBb9ehOoKG9UE+HAE1YvH1XX2XVGeEqYUY-Tsk7YBTz0WpSpoYyPgx6Iki5KLtQ5G-aKP9eysnkuOAkrvHU8bLbGtZteGwJarev03PhfCioJL4OSqsmQGEvDbHFEbNl1qJtdwEriR+VNZts9vNNLk7UGfeNwIiqpxjk4Mn09nmSd8FhM4ifvcaIbNCRoMPGl6KU12iseSe+w+1kFsLhX+OhQM8WXcWV10cGqBzQE9OqOLUcg9n0krrR3KrohstS9smTwEx9olyLYppvC0p5i7dAx2deWvM1ZxKNs0BvcXGukR+/g" BCompare
# 然后打开Beyond Conpare，弹出Trial Mode Error！弹窗，单击右下角按钮“Enter Key”，输入以下秘钥【注意：包括开始和结尾的横线行】 
--- BEGIN LICENSE KEY ---
GXN1eh9FbDiX1ACdd7XKMV7hL7x0ClBJLUJ-zFfKofjaj2yxE53xauIfkqZ8FoLpcZ0Ux6McTyNmODDSvSIHLYhg1QkTxjCeSCk6ARz0ABJcnUmd3dZYJNWFyJun14rmGByRnVPL49QH+Rs0kjRGKCB-cb8IT4Gf0Ue9WMQ1A6t31MO9jmjoYUeoUmbeAQSofvuK8GN1rLRv7WXfUJ0uyvYlGLqzq1ZoJAJDyo0Kdr4ThF-IXcv2cxVyWVW1SaMq8GFosDEGThnY7C-SgNXW30jqAOgiRjKKRX9RuNeDMFqgP2cuf0NMvyMrMScnM1ZyiAaJJtzbxqN5hZOMClUTE+++
--- END LICENSE KEY -----
```

[参考]: https://blog.csdn.net/qq_26012495/article/details/86514147



# Git

```
sudo apt-get install git
```



# RAR

```
sudo apt-get install rar
```

