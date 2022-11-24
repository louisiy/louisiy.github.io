---
title: 杂记录
date : 2022-11-24
tag: record&reference
---


# 常见小问题

## 终端无法运行脚本

参考[Microsoft文档](https:/go.microsoft.com/fwlink/?LinkID=135170) 中about_Execution_Policies



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



## 建站

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

## 添加404页面

(这里为暂时收录的参考)

原来的主题没有404页面，加一个也不是什么难事。首先在/source/目录下新建一个404.md，内容如下：

```
title: 404
date: 2019-07-19 16:41:10
type: "404"
layout: "404"
description: "你来到了没有知识的荒原 :("
```

然后在/themes/matery/layout/目录下新建一个404.ejs文件，内容如下：
```ejs
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 75vh;
    }
</style>

<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

## 图床

采用

 jsdelivr (cdn) + github 存储 + PicGo 

方案

// load any GitHub release, commit, or branch

// note: we recommend using npm for projects that support it

https://cdn.jsdelivr.net/gh/user/repo@version/filecdn+



## 遇到的一些问题

1 Q&E

```bash
platform unsupported hexo-deployer-git@3.0.0 › hexo-fs@3.1.0 › chokidar@3.5.3 › fsevents@~2.3.2 Package require os(darwin) not compatible with your platform(win32)
[fsevents@~2.3.2] optional install error: Package require os(darwin) not compatible with your platform(win32)
```

A 这是一个可以忽略的错误，fsevents是可选的依赖，只能应用于maxOS系统，不适合Windows或者Linux，也就是忽略即可
