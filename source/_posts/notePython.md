---
title: Notes for SummerSchool Python
date: 2023-08-13 22:25:57
tags: python
---

## 环境前提

Anaconda是一个用于科学计算的Python发行版，支持Linux，Mac，Windows系统，提供了包管理与环境管理的功能，可以很方便地解决多版本python并存，切换以及各种第三方包安装问题

anaconda利用工具/命令`conda`来进行package和environment的管理，并且已经包含了Python和相关的配套工具

### 概念

Anaconda是一个打包的集合，预装了conda，某个版本的python，很多package，科学计算工具等，称为一个发行版

conda是一个工具，也是一个可执行命令，核心功能是包管理和环境管理。包管理与pip的使用类似，环境管理则允许用户方便地安装不同版本的python并可以快速切换

Miniconda是一个简单包，只包含最基本的内容，python和conda，以及相关的必须依赖项

## 基础语法

- 交互式编程
  - 交互式编程不需要创建脚本文件，是通过Python解释器的交互模式来编写代码
- 脚本式编程
  - 通过脚本参数调用解释器开始执行脚本，知道脚本执行完毕。当脚本执行完成后，解释器不再有效

### 标识符

英文字母、数字、下划线组成，不能以数字开头

区分大小写

以下划线开头的标识符是有特殊意义的

Python可以同一行显示多条语句，方法是用分号；分开，如

```python
>>> print("hello");print("Tsinghua");
hello
Tsinghua
```

保留字

```python
import keyword
keyword.kwlist
```

查看当前系统的关键字

| and      | exec    | not    |
| -------- | ------- | ------ |
| assert   | finally | or     |
| break    | for     | pass   |
| class    | from    | print  |
| continue | global  | raise  |
| def      | if      | return |
| del      | import  | try    |
| elif     | in      | while  |
| else     | is      | with   |
| except   | lambda  | yield  |

### 行和缩进

用缩进来写模块

缩进的空白数量是可变的，但是所有代码快语句必须包含相同的缩进空白数量

Indentation Error: unindent does not match any outer indentation level

### 引号和注释

可以用引号（'）、双引号( " )、三引号( ''' 或 """ ) 来表示字符串，引号的开始与结束须是相同类型

单行注释采用 # 开头。多行注释使用三个单引号 ''' 或三个双引号 """

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。空行与代码缩进不同，空行并不是Python语法的一部分

### print输出

print默认输出是换行的

可以在 print() 函数中添加 end="" 参数，这样就可以实现不换行效果

参数 end 默认值为 "\n"，即end="\n"，表示换行，给 end 赋值为空, 即end=""，就不会换行了

### 变量类型

变量赋值不需要类型声明。
每个变量在内存中创建，都包括变量的标识，名称和数据这些信息
每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建
等号 = 用来给变量赋值
等号 = 运算符左边是一个变量名，等号 = 运算符右边是存储在变量中的值

```python
a=b=c=1
a,b,c=1,2,"john"
```

五个标准数据类型

- Numbers（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Dictionary（字典）
