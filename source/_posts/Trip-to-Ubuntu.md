---
title: Trip to Ubuntu
date: 2020-03-20 21:00:00
tags: Ubuntu
---



## Trip to Ubuntu

### 前言

前几天我在Microsoft Store里发现了Ubuntu，它是运行在Windows上的子系统，第一时间我就开始了Ubuntu的配置探索之旅。

### 安装

我在Microsoft Store安装之后，发现它是装在C盘系统盘的，为了节省C盘空间，我采取了转移到非系统盘的方式

通过安装，我定位到了它的安装路径

```
C:\Users\xxxx\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc
```

接着，我将它卸载，通过创建软链接来解决这个问题

先在想要安装的位置创建了Ubuntu文件夹

然后打开`cmd`终端，输入

```bash
mklink /j C:\Users\XXXX\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc  D:\Ubuntu\
```

创建成功后，再重新在Microsoft Store中安装Ubuntu

安装过程中如果出现了安装失败的问题，可以运行

```bash
icacls D:\Ubuntu /grant "用户名:(OI)(CI)(F)"
```

### 配置

#### 换源

清华源

- 网址：[https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
- 源地址：[https://mirrors.tuna.tsinghua.edu.cn/ubuntu/](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/ubuntu/)

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

阿里源

- 网址：[https://opsx.alibaba.com/guide?lang=zh-CN&document=69a2341e-801e-11e8-8b5a-00163e04cdbb](https://link.zhihu.com/?target=https%3A//opsx.alibaba.com/guide%3Flang%3Dzh-CN%26document%3D69a2341e-801e-11e8-8b5a-00163e04cdbb)
- 源地址：[http://mirrors.aliyun.com/ubuntu/](https://link.zhihu.com/?target=http%3A//mirrors.aliyun.com/ubuntu/)

```
# 默认注释了源码仓库，如有需要可自行取消注释
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```

中科大源

- 网址：[http://mirrors.ustc.edu.cn/help/ubuntu.html](https://link.zhihu.com/?target=http%3A//mirrors.ustc.edu.cn/help/ubuntu.html)
- 源地址：[https://mirrors.ustc.edu.cn/ubuntu/](https://link.zhihu.com/?target=https%3A//mirrors.ustc.edu.cn/ubuntu/)

```
# 默认注释了源码仓库，如有需要可自行取消注释
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

网易源

- 网址：[http://mirrors.163.com/.help/ubuntu.html](https://link.zhihu.com/?target=http%3A//mirrors.163.com/.help/ubuntu.html)
- 源地址：[http://mirrors.163.com](https://link.zhihu.com/?target=http%3A//mirrors.163.com/)

```
# 默认注释了源码仓库，如有需要可自行取消注释
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
```

先对系统给的镜像源进行备份，以防止出现问题

```bash
$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup 
```

然后，修改数据源配置文件

```bash
$ sudo vim /etc/apt/sources.list
```

选择一个源添加到文件最前面或直接将官方的源注释掉替换掉原文件

更新软件源中的所有软件列表：

```bash
$ sudo apt-get update
```

更新软件：

```bash
$ sudo apt-get upgrade
```

更新系统版本：

```bash
$ sudo apt-get dist-upgrade
```

下载中文语言包：

```bash
$ sudo apt-get install language-pack-zh-han*
```

#### 安装 C/C++ 开发环境

安装编译工具包：

```bash
$ sudo apt-get install build-essential
```

安装 nginx 依赖库：

```bash
$ sudo apt-get install libpcre3 libpcre3-dev libpcrecpp0 libssl-dev zlib1g-dev
```

#### 图形界面

首先安装使用VcXsrv Windows X Server

启动Launcher，首次启动自动进入界面设置后，选择：“one large window”，Display number设置成0，其它默认即可：

**安装桌面环境**

```bash
sudo apt-get install ubuntu-desktop unity compizconfig-settings-manager
```

启动之前安装的X-Windows，在Bash中执行如下命令：

```bash
export  DISPLAY=localhost:0
ccsm
```

**启动compiz (打开桌面)**

```bash
compiz
```
