---
title: 来到Win11
date: 2023-08-24 14:12:00
tags: Windows
---

## 奇幻之旅

最近把2020款的拯救者从win10升级到了win11，界面体验很新，但是还是有诸多不习惯的地方

## 反人类的右键

通过修改注册表，我们就可以回退到老版右键菜单

运行`regedit`，开启注册表编辑器，定位到`HKEY_CURRENT_USER\SOFTWARE\CLASSES\CLSID`

右键点击`CLSID`键值，新建一个名为`{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}`的项

右键点击新创建的项，新建一个名为`InprocServer32`的项，按下回车键保存

最后选择新创建的项，然后双击右侧窗格中的默认条目，什么内容都不需要输入，按下回车键

保存注册表后，重启`explorer.exe`，即可看到右键菜单恢复成旧样式了。

如果想要恢复成为Win11的设计，那么删掉`InprocServer32`的项就可以了。

圆角设计的老版本右键菜单还是蛮好看的

## 消失的开始菜单磁贴

安装了破解版本的startdock11，还原了win10风格的磁贴，观感和使用好了太多了

## 别扭的显示放大比例

125%虽然是推荐，但是感觉太大了

100%的大小很舒服，但是有点模糊，又蛮难受

## 太宽的任务栏

老规矩，进注册表

`\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced`

右击空白处并新建一个TaskbarSi”的DWORD值（32）

修改其值为0（小）1（中）2（大）
