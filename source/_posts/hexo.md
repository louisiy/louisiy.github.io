---
title: Hexo建站
date: 2023-02-07 19:23:24
tags: hexo
---

采用Hexo建立个人博客，本文记录建站经历

## 建站思路

hexo+github+vercel+godaddy+dnspod

框架+GitHub存储 +网站托管服务 +域名注册+域名服务器DNS

先用着

上面已经备份之后，再继续进行操作就只需在hexo_src分支下进行

hexo的操作，以及git操作

批处理

```bash
hexo clean;hexo g;hexo d;git status ;git add . ;git commit -m 'new push';git push origin hexo_src
```

挺方便

## Hexo

使用 `hexo init` 来生成基本文件，做一些基本的配置，像安装模块、配置主题等

使用 `hexo new page / post`来生成文章或页面的 md 文件，编辑

使用 `hexo g` 生成文件，Hexo 会根据主题中的模板，来生成对应的 html 文件，转译 CSS 文件，复制其它的静态文件（如图片图标字体等），组织为一个静态网站

使用 `hexo d` 来部署，一般是借助一些部署模块完成

## 备份博客源文件

有时候我们想换一台电脑继续写博客，这时候就可以将博客目录下的所有源文件都上传到github上面。

### 备份

首先在github博客仓库下新建一个分支`hexo_src`，然后`git clone`到本地，把`.git`文件夹拿出来，放在博客根目录下。

然后`git checkout -b hexo_src`切换到`hexo_src`分支，然后`git add .`，然后`git commit -m "xxx"`，最后`git push origin hexo_src`提交就行了。

### 恢复

首先在指定文件夹clone下来hexo_src分支

```bash
$ git clone -b hexo_src [仓库地址]
```

在该clone下来的文件夹里去安装hexo

```bash
$ cnpm install hexo
$ cnpm install		//这一句不知道和上面这一句有何区别
```

> 特别注意，hexo_src中的theme文件夹在上传分支后为空，需要再次从原主题仓库clone下来处理

## 图床

采用

 jsdelivr (cdn) + github 存储 + PicGo 

方案

// load any GitHub release, commit, or branch

// note: we recommend using npm for projects that support it

https://cdn.jsdelivr.net/gh/user/repo@version/filecdn+