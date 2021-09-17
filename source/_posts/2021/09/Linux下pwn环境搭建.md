---
title: Linux下pwn环境搭建
catalog: true
date: 2021-09-17 21:59:12
subtitle:
tags:
- 二进制
- pwn
- linux
categories:
- 技术
lang: en
header-img: /rain_and_tyo/img/titles_img/hello_world_bg.jpg
---
​
# 0. 环境准备
> Ubuntu 20.04 
>
> x86_64
>
> python3.8.*
>
> gdb 9.2

# 1. 安装列表
|pwntools|python的CTF框架和漏洞利用开发库|
|--|--|
|checksec|可以查看二进制文件开启了哪些保护机制|
|ropgadget|寻找合适的 gadgets，在编ROP exp 的时候有很大作用|
|pwndbg|gdb的插件，在ctf中pwn题目中使用非常方便，依赖python|
|tmux|是一个终端复用器，结合gdb使用，可以方便调试python脚本|
|Ghidra|开源的软件逆向工具|

# 2. 开始安装
1）安装pwntools：
直接使用pip安装：
```bash
pip install pwntools
```
**注意**：如果设备上同时存在python2和python3注意区别，这里要下载的是python3的

2）安装checksec、ropgadget：
安装好pwntools应该已经装好这两个工具了，可以直接输入checksec、ROPgadget命令试一下：
![new world](1.png)

如果没有，可以尝试使用pip install直接安装：
```bash
pip install checksec
pip install ropgadget
```
3）安装pwndbg：
github链接：
https://github.com/pwndbg/pwndbg

直接下载最新版本，参考reademe的操作进行安装：
```bash
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```
安装需要依赖python环境，如果apt-get install命令执行有报错可以尝试手动复制脚本的代码执行一下，可能存在安装的组件名字不一致导致命令执行失败的情况，这种情况可以直接注释掉setup.sh脚本里报错的apt-get install操作，直接往后执行。

安装完成后，会有一个/root/.gdbinit生成，因为只在root目录下有，所以目前只有root用户执行gdb的时候才会加载该插件，如果想用非root用户，则可以把.gdbinit文件复制到相应用户的根目录下面，比如复制到/home/test目录，则test用户执行gdb也可以加载pwndbg插件（注意，要确保非root用户有执行.gdbinit文件中指向的python脚本的权限，如果没有权限则需要添加权限）
插件激活成功的效果如下：
![new world](2.png)

4）安装tmux:
直接apt安装即可：apt install tmux，结合pwntool和gdb的使用方法可以参考如下文章：
[pwntools的使用技巧](https://tianstcht.github.io/pwntools%E7%9A%84%E4%BD%BF%E7%94%A8%E6%8A%80%E5%B7%A7/)

5）安装Ghidra：
github链接：
https://github.com/NationalSecurityAgency/ghidra

参考readme安装即可，提供了两种安装方式，一种是直接安装编好的二进制，另一种是源码编译。

源码编译可参考如下说明：
https://github.com/NationalSecurityAgency/ghidra/blob/master/DevGuide.md

Ghidra是java编写的，需要安装JDK，linux平台JDK安装可以参考如下：

[linux配置java环境变量(详细)](https://www.cnblogs.com/samcn/archive/2011/03/16/1986248.html)

windows平台安装JDK直接下载msi文件即可

​