---
title: GNU
date: 2023-1-30 17:55:33
tags: GNU
---



最近更新MinGW，并且由于学习原因需要Cygwin，下载过程中发现很多疑惑。故摘录了部分关于GNU以及相关开发的信息和后面补充的Unix、Linux的内容。信息源有Wikipedia、官网等。

## GNU计划

**GNU计划**（英语：**GNU Project**），又译为**革奴计划**，是一个自由软件计划，1983年9月27日由理查德·斯托曼在麻省理工学院公开发起。它的目标是创建一套完全自由的操作系统，称为GNU。

理查德·斯托曼最早在net.unix-wizards新闻组上公布该消息，并附带一份《GNU宣言》等解释为何发起该计划的文章，其中一个理由就是要“重现当年软件界合作互助的团结精神”。

由于GNU将要实现UNIX系统的接口标准，因此GNU计划可以分别开发不同的操作系统。GNU计划采用了部分当时已经可自由使用的软件，例如TeX排版系统和X Window视窗系统等。不过GNU计划也开发了大批其他的自由软件，这些软件也被移植到其他操作系统平台上，例如Microsoft Windows、BSD家族、Solaris及Mac OS。

为保证GNU软件可以自由地“使用、复制、修改和发布”，所有GNU软件都包含一份在禁止其他人添加任何限制的情况下，授权所有权利给任何人的协议条款，GNU通用公共许可证（GNU General Public License，GPL）。这个就是被称为“公共著作权”的概念。GNU也针对不同场合，提供GNU宽通用公共许可证与GNU自由文档许可证这两种协议条款。

## GNU

GNU是一个自由的操作系统，其内容软件完全以GPL方式发布。

作为操作系统，GNU的发展仍未完成，其中最大的问题是具有完备功能的内核尚未被开发成功。GNU的内核，称为Hurd，是自由软件基金会发展的重点，但是其发展尚未成熟。在实际使用上，多半使用Linux内核、FreeBSD等替代方案，作为系统核心，其中主要的操作系统是Linux的发行版。Linux操作系统包涵了Linux内核与其他自由软件项目中的GNU组件和软件，可以被称为GNU/Linux。

## GCC

GNU编译器套装（英语：GNU Compiler Collection，缩写为GCC）是GNU计划制作的一种优化编译器，支持各种编程语言、操作系统、计算机系统结构。该编译器是以GPL及LGPL许可证所发行的自由软件，也是GNU计划的关键部分，还是GNU工具链的主要组成部分之一。GCC（特别是其中的C语言编译器）也常被认为是跨平台编译器的事实标准。1985年由理查德·马修·斯托曼开始发展，现在由自由软件基金会负责维护工作。截至2019年，GCC大约有1500万行代码，是现存最大的自由程序之一。它在自由软件的发展中发挥了重要作用，不仅是一个工具，还是一个典例。

原名为GNU C语言编译器（GNU C Compiler），因为它原本只能处理C语言。同年12月，新的GCC编译器可以编译C++语言。后来又为Fortran、Pascal、Objective-C、Java、Ada，Go等其他语言开发了前端。C和C++编译器也支持OpenMP和OpenACC规范。

GCC编译器已经被移植到比其他编译器更多的平台和指令集架构上，并被广泛部署在开发自由和专有软件的工具中。GCC还可用于许多嵌入式系统，包括基于ARM和Power ISA的芯片。

GCC不仅是GNU操作系统的官方编译器，还是许多类UNIX系统和Linux发行版的标准编译器。BSD家族中的大部分操作系统也在GCC发布之后转用GCC；不过FreeBSD、OpenBSD和Apple macOS已经转向了Clang编译器，主要是因为许可问题。GCC也可以编译Windows、Android、iOS、Solaris、HP-UX、IBM AIX和DOS系统的代码。GCC原本用C开发，后来因为LLVM、Clang的崛起，它更快地将开发语言转换为C++。许多C的爱好者在对C++一知半解的情况下主观认定C++的性能一定会输给C，但是Ian Lance Taylor给出了不同的意见，并表明C++不但性能不输给C，而且能设计出更好，更容易维护的程序。

## Cygwin

Cygwin是许多自由软件的集合，最初由Cygnus Solutions开发，用于各种版本的Microsoft Windows上，运行类UNIX系统。Cygwin的主要目的是通过重新编译，将POSIX系统（例如Linux、BSD，以及其他Unix系统）上的软件移植到Windows上。Cygwin移植工作在Windows NT上比较好，在Windows 95和Windows 98上，相对差劲一些。目前Cygwin由Red Hat等负责维护。

## MinGW

MinGW（Minimalist GNU for Windows），又称mingw32，是将GCC编译器和GNU Binutils移植到Win32平台下的产物，包括一系列头文件（Win32API）、库和可执行文件。

另有可用于产生32位及64位Windows可执行文件的MinGW-w64项目，是从原本MinGW产生的分支。如今已经独立发展。

## MinGW-w64

Mingw-w64是自由及开放源代码软件开发环境，用于创建Microsoft Windows应用程序。从2005–2008从MinGW(Minimalist GNU for Windows)分支出来。

Mingw-w64包括对GCC、GNU Binutils的Windows版本的移植（汇编器、链接器、库文件管理器），一套自由可分发的Windows特定的头文件与静态导入库以使用Windows API，一个 Windows本地版本的GNU的调试器，以及其它多种工具。

Mingw-w64可运行于本地Microsoft Windows平台，"cross-native"在MSYS2或Cygwin。Mingw-w64能生成32-或64-位可执行程序，运行于i686-w64-mingw32或x86_64-w64-mingw32目标平台

## MSYS2

MSYS2 ("minimal system 2")是用于Microsoft Windows的软件发布与开发平台，基于Mingw-w64与Cygwin，把Unix环境中的代码移植到Windows。
