---
title: My Raspberry Pi
date: 2021-02-13 13:02:00
tags: Raspberry Pi
---





## About Raspberry Pi

这里本来打算放张图片


## 启动

### 最理想情况

官网下载Pi OS镜像，烧录SD卡，连接好键盘显示屏，启动

### 无显示屏

- 开启ssh访问，在boot目录新建`ssh`的不带后缀名的空文件。
- 用RJ45网线连接笔记本电脑和树莓派。
- 等待树莓派启动完毕，打开笔记本电脑的网络和共享中心，可以看到出现了一个未识别网络，连接方式是以太网。然后再查看分配给这个以太网的接口IP地址（如192.168.137.1）
- 手机开热点，通过它可以让树莓派联网。在网络和共享中心中，点击无线网络->属性->共享 ，给“允许其它网络用户通过此计算机的internet来连接”打勾，然后确定。
- 打开CMD，输入以下命令查看网络接口信息：`arp -a`
- 在前面查到的接口：192.168.137.1 下方找到第一个连接的ip地址，即为树莓派的ip地址。
- 打开PuTTY
  - host 填入: 开发板 ip 即可。
  - 用户名、密码同串口登陆一致（默认：pi、raspberry）

## 配置

### 更新软件源

首先查看自己树莓派系统版本，一般有jessie,stretch,buster,wheezy这4个版本。

```
No LSB modules are available.
Distributor ID:	Raspbian
Description:	Raspbain GNU/Linux 10 (buster)
Release:		10
Codename: 		buster
Copy
```

开始换源

```
sudo vim /etc/apt/sources.list
Copy
```

将默认的内容删掉或者用`#`号注释，改为

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/   buster main contrib non-free rpi

deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
Copy
```

或者用以下地址代替上面的地址栏

中山大学
[Raspbian http://mirror.sysu.edu.cn/raspbian/raspbian/](https://link.zhihu.com/?target=http://mirror.sysu.edu.cn/raspbian/raspbian/)

中国科学技术大学
[Raspbian http://mirrors.ustc.edu.cn/raspbian/raspbian/](https://link.zhihu.com/?target=http://mirrors.ustc.edu.cn/raspbian/raspbian/)

清华大学
[Raspbian ](https://link.zhihu.com/?target=http://mirrors.tuna.tsinghua.edu.cn/raspbian/)[http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/](https://link.zhihu.com/?target=http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/)

华中科技大学
[Raspbian http://mirrors.hustunique.com/raspbian/raspbian/](https://link.zhihu.com/?target=http://mirrors.hustunique.com/raspbian/raspbian/)

[Arch Linux ARM http://mirrors.hustunique.com/archlinuxarm/](https://link.zhihu.com/?target=http://mirrors.hustunique.com/archlinuxarm/)

大连东软信息学院源（北方用户）

[Raspbian http://mirrors.neusoft.edu.cn/raspbian/raspbian/](https://link.zhihu.com/?target=http://mirrors.neusoft.edu.cn/raspbian/raspbian/)

（可参考Ubuntu配置）

再更新

```
sudo apt update
Copy
```

更新系统软件

```
sudo apt upgrade
Copy
```

## Web项目

### LNSP

Linux Nginx SQLite PHP

```
sudo apt install nginx
sudo apt install php7.3
sudo apt install php7.3-fpm
sudo apt install php7.3-sqlite
sudo apt install php7.3-common
sudo apt install sqlite
Copy
```

- 启动 nginx

```
sudo /etc/init.d/nginx start
Copy
```

- 修改 nginx 的配置文件

```
sudo vim /etc/nginx/sites-available/default
Copy
```

- PHP 脚本支持

找到 php 的定义段，将这些行的注释去掉 ，修改后内容如下

```
# Default server configuration
#
server {
        listen 80 default_server;
        listen [::]:80 default_server;
 
        root /var/www/html;
 
        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html index.php;
 
        server_name _;
 
        location / {
                 # First attempt to serve request as file, then
                 # as directory, then fall back to displaying a 404.
                 try_files $uri $uri/ =404;
        }
 
        # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ {
                 # include snippets/fastcgi-php.conf;
                 #
                 # # With php-fpm (or other unix sockets):
                 fastcgi_pass unix:/run/php/php7.3-fpm.sock;
                 # # With php-cgi (or other tcp sockets):
                 # fastcgi_pass 127.0.0.1:9000;
                 # 设置脚本文件请求的路径
                 fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                 # 引入fastcgi的配置文件 
                 include fastcgi_params;
        }
 
}
Copy
```

- 重新加载 nginx 的配置

```
sudo /etc/init.d/nginx reload
Copy
```

- 测试 html

通过主机的 IE 访问树莓派，可以看到主页 (表示 Web 服务器已正常启动)

- 测试 php

输入下列命令

```
sudo chmod 777 /var/            #下面三行给文件授予权限
sudo chmod 777 /var/www
sudo chmod 777 /var/www/html
Copy
```

在树莓派中生成一`php`文件

```
sudo vim /var/www/index.php
Copy
```

在文件中输入以下内容

```
<?php  
  print <<< EOT  
<!doctype html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<title>Test successful</title>  
</head>  
<body>  
<h1>Test successful</h1>  
<p>Congratulations.</p>  
<p>Your webserver and PHP are working.</p>  
</body>  
</html>  
EOT;  

?>
Copy
```

到此为止，lnsp就安装完毕

### Node.js

官方

```
wget https://nodejs.org/dist/v12.16.1/node-v00.00.0-linux-armv7l.tar.xz
Copy
```

国内镜像

```
wget https://npm.taobao.org/mirrors/node/latest/node-v00.00.0-linux-armv7l.tar.xz
Copy
```

解压：

```
xz -d node-v12.13.1-linux-armv7l.tar.xz
tar -xavf node-v12.13.1-linux-armv7l.tar
Copy
```

将系统内原本存在的`/usr/bin.node`删除

```
sudo rm -rf /usr/bin/node
#如果不存在，忽略此步骤
Copy
```

解压后，将二进制包移动到`/usr/local/node`下

```
sudo mv ./node-v10.0.0-linux-armv7l /usr/local/node
Copy
```

为`node`和`npm`建立软连接，在终端输入：

```
sudo ln -s /usr/local/node/bin/node /usr/bin/node
sudo ln -s /usr/local/node/bin/npm /usr/bin/npm
#这类似于Windows中的快捷方式
Copy
```

通过查看`node`和`npm`版本的方式来查看是否成功

```
node -v && npm -v
Copy
```

可以看到对应的版本号说明安装成功

由于国内网速问题`npm`包管理器的速度会较慢，利用`npm`安装`cnpm`某宝源

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
Copy
```

为`cnpm`建立软连接

```
sudo ln -s /usr/local/node/bin/cnpm /usr/bin/cnpm
Copy
```

> Windows cnpm
>
> ```
> $ npm install cnpm -g
> Copy
> ```
>
> If you’re in China, maybe you should install it from our [China mirror](https://npm.taobao.org/):
>
> ```
> $ npm install cnpm -g --registry=https://r.npm.taobao.org
> Copy
> ```

### Hexo

至于软件，暂时选用Hexo，虽然还有Typecho、WordPress备选，但是先尝试这个。

主题参考

- [fluid](https://github.com/fluid-dev/hexo-theme-fluid)（主要看上了打字机特效）
- [Aomori](https://linhong.me/2020/01/27/hexo-theme-aomori/)（看上了右边栏特效）
- [Ieo](https://github.com/lixuetaoleo/hexo-theme-leo)（封面）
- [Edinburgh](https://sharvaridesai.github.io/hexo-theme-edinburgh-demo/)（看设计感）
- [mrwill](https://mrwillcom.github.io/)（设计感）

### 方案

在本地完成hexo渲染，部署public文件夹到树莓派，完整文件夹git到github仓库。

## 附录

### Linux 常用命令

文件改名 `sudo mv test.txt new.txt\`

- **mkdir xxx** 创建文件夹xxx
- **mkdir a1 a2 a3** 批量创建文件夹 a1、文件夹 a2、文件夹 a3
- **mkdir -p b1/b2/b3** 连续创建文件夹 b1、文件夹 b2、文件夹 b3

### RPI串口电路