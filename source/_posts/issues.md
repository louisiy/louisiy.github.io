---
title: 杂记录
date : 2022-8-13
tag: record&reference
---



# 常见小问题

## 终端无法运行脚本

参考[Microsoft文档](https:/go.microsoft.com/fwlink/?LinkID=135170) 中about_Execution_Policies



## 备份博客源文件

有时候我们想换一台电脑继续写博客，这时候就可以将博客目录下的所有源文件都上传到github上面。

首先在github博客仓库下新建一个分支`hexo_src`，然后`git clone`到本地，把`.git`文件夹拿出来，放在博客根目录下。

然后`git checkout -b hexo_src`切换到`hexo_src`分支，然后`git add .`，然后`git commit -m "xxx"`，最后`git push origin hexo_src`提交就行了。



## 建站

hexo+github+vercel+godaddy+dnspod

框架+GitHub pages +框架 +域名注册+域名服务器

先用着

上面已经备份之后，再继续进行操作就只需在hexo_src分支下进行

hexo的操作，以及git操作

挺方便