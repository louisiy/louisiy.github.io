---
title: Mermaid
date: 2023-02-04 09:35:00
tags: mermaid
---


mermaid是一款支持在Markdown文档中使用的图表工具，可以用来画时序图，类图，流程图等。在Markdown中使用十分方便，编辑器Typora支持mermaid。mermaid是基于javascript实现的，将Markdown文档中的元素渲染成HTML元素。基于white搭建的博客起初并不支持mermaid，记录一下实现过程。

1. 安装插件

   ```bash
   $ npm install --save hexo-filter-mermaid-diagrams
   ```

2. 修改主题的配置文件`_config.yml`

   ```yaml
   # Mermaid (markdown to flow chart, seq chart, class chart ...)
   mermaid: 
   	enable: true
   	# Available themes: default | dark | forest | neutral
   	theme: default
   ```

3. 下载js文件

   ``