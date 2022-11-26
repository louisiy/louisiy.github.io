---
title: 游玩LaTeX
date: 2022-11-23 14:01:27
tags: LaTeX
---

# LaTeX笔记

## 快速入门

```latex
//English
\documentclass{article}
\begin{document}
"Hello world!" from \LaTeX
\end{document}

//中文
\documentclass{ctexart}
\begin{document}
“你好，世界！” 来自 \LaTeX{} 的问候  //怀疑这里新版本不需要{}占空格了
\end{document}
```

## 相关概念辨析

### 引擎

排版引擎是编译源代码并生成文档的程序，如pdfLaTeX、XeLaTeX等。有时也称为编译器

### 格式

是定义了一组命令的代码集。如LaTeX。高纳德本人也编写了一个简单的plain TeX格式，但仍不便于使用

### 编译命令

是实际调用的、结合了引擎和格式的命令。如`xelatex`命令是结合了XeTeX引擎和LaTeX格式的一个编译命令