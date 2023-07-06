---
title: Notes for Nginx
date: 2023-07-06 13:14:57
tags: nginx
---



## 前言

C10k = 10000 concurrent connection处理10000个并发连接

## 安装

### 包管理器安装

```bash
//Linux apt yum
$ sudo apt update
$ sudo apt install nginx
//Mac Homebrew
$ brew install nginx
//Windows scoop chocolatey
$ scoop install nginx
$ choco install nginx
```

### 编译安装

```bash
$ ./configue											//预编译
	--sbin-path=/usr/local/nginx/nginx
	--conf-path=/usr/local/nginx/nginx.conf
	--pid-path=/usr/local/nginx/nginx.pid
	--with-http_ssl_module
	--with-pcre=../pcre2-10.39
	--with-zlib=../zlibb-1.2.11
$ make												  //编译
$ make install										       //安装
```

### 使用Docker安装

```bash
$ docker pull nginx
```

## 服务启停

启动服务

```bash
$ nginx
```

没有提示消息即表明顺利启动，启动失败会有提示

浏览器输入

```http
localhost
```

查看到默认欢迎页面即可

在命令行输入

```bash
$ ps -ef|grep nginx
```

可以查看Nginx的进程

Nginx的进程模型为master为主进程，负责读取和验证配置文件以及管理worker进程，worker进程为工作进程，负责处理实际的请求，worker文件可以有多个

```bash
$ lsof -i:80		//查看端口的利用情况		后缀为只查看的端口
```

可以用如下命令停止服务器

```bash
$ nginx -s stop
```

## 静态站点部署

```bash
$ nginx -V
```

可以用来查看安装目录，编译参数等各种信息

也可以通过

```bash
$ nginx -t
```

来查看配置文件的位置

放在默认文件夹下即可

## 配置文件

修改配置文件后，需要使用命令来检查配置文件是否正确

```bash
$ nginx -t
```

worker_processes进程数量，保持同服务器CPU内核的数量相同是比较合适的，也可以设置为auto来自动配置进程数量

### 结构

```
worker_processes auto;						   //全局块	设置

events {									//events块	服务器和客户端网络连接配置
	worker_connections 1024;
}

http {										//http块
	include			  mime.types;
	default_type	    application/octet-stream;
	sendfile		   on;
	keepalive_timeout  65;
	
	server {								////多个server块
		listen		  80;
		server_name	 localhost;
		return 301	  https://$server_name$request_uri;
	}
	
	server {
		listen		  8000;
		listen		  somename:8080;
		server_name	 somename	alias	another.alias;
		location / {
			root	html;
			index	index.html index.htm;
		}
	}
	include servers/*;			//把servers目录下的所有配置文件都包含进来
}
```

## 反向代理

代理服务端

```
upstream backend{
	ip_hash;
	server 127.0.0.1:8000	weight = 3;
	server 127.0.0.1:8001;
	server 127.0.0.1:8002;
}

server {
	location /app{
		proxy_pass http://backend;
	}
}
```

修改完需重载配置文件

```bash
$ nginx -s reload
```

默认轮询的方式来代理

可以使用权重weight来分配负载均衡

ip_hash策略来使同一个客户端的请求分配到同一台服务器上，解决一些session的问题

## https配置

http默认端口80，https默认端口443

> 使用openssl生成证书
>
> 生成私钥文件（private key）
>
> ```
> openssl genrsa -out private.key 2048
> ```
>
> 根据私钥生成证书签名请求文件
>
> ```
> openssl req -new -key private.key -out cert.csr
> ```
>
> 使用私钥对证书申请进行签名从而生成证书文件（pem文件）
>
> ```
> openssl x509 -req -in cert.csr -out cacert.pem -signkey private.key

在配置文件中进行配置

```
server{
	listen 80;
	server_name adf;
	return 301 https://$server_name$request_url;
}

server{
	listen 443 ssl;
	server_name locahost;	//一般填写自己的域名
	
	ssl_certificate			路径;
	ssl_certificate_key		 路径;
	
	//加密协议和算法相关的配置
	ssl_session_timeout 	5m;	//缓存有效期
	ssl_protocols	TLSv1	TLSv1.1	TLSv1.2	TLSv1.3;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:! aNULL:! MD5:!RC4:!DHE;
	ssl_prefer_server_ciphers on;
}
```

## 配置文件

通过server来设置虚拟主机

可以在/server下新建文件，然后重载也可以

```
# vue.conf

server{
	listen 5173;
	server_name locahost;
	location /{
		root	路径;
		index	index.html index.htm;
	}
}
```