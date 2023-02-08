---
title: 杂记录
date : 2022-11-24
tag: record&reference
---


# 常见小问题

## 终端无法运行脚本

参考[Microsoft文档](https:/go.microsoft.com/fwlink/?LinkID=135170) 中about_Execution_Policies

## 遇到的一些问题

1 Q&E

```bash
platform unsupported hexo-deployer-git@3.0.0 › hexo-fs@3.1.0 › chokidar@3.5.3 › fsevents@~2.3.2 Package require os(darwin) not compatible with your platform(win32)
[fsevents@~2.3.2] optional install error: Package require os(darwin) not compatible with your platform(win32)
```

A 这是一个可以忽略的错误，fsevents是可选的依赖，只能应用于maxOS系统，不适合Windows或者Linux，也就是忽略即可
