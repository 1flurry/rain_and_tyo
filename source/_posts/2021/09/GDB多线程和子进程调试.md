---
title: GDB多线程和子进程调试
catalog: true
date: 2021-09-15 23:50:23
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
# 多线程调试常用命令
|命令|描述|
|--|--|
|info threads|显示所有thread线程，有 * 的表示当前线程|
|thread thread_nums|gdb线程切换到对应thread|
|thread apply ID1 ID2...... command|让一个或者多个线程执行GDB命令command|
|thread apply all command|让所有线程执行GDB命令command|
|b xxxx thread thread_nums|给thread_nums线程的xxxx函数打断点|
|set scheduler-locking off/on/step|off：不锁定任何线程，也就是所有线程都执行，默认值；on：只有当前被调试程序会执行；step：当使用step命令调试线程时，其他线程不会执行，但使用其他命令（如next）调试线程时，其他线程会执行|
|thread apply all bt|查看所有线程的调用栈|
# 多线程调试辅助命令
|命令|描述|
|--|--|
|gdb -batch -ex 'thread_nums' -ex 'bt' -batch -p pid|输出指定线程的调用栈并立即退出gdb（防止进程挂死）|
|find /proc/pid/task/ -name "sched" \| xargs grep threads  |获得pid对应进程的所有任务tid|
|gdb -batch -ex 'bt' -p tid|获取tid对应线程的调用栈并立即退出gdb（如果不知道thread_nums可使用这种方法）|
# 子进程调试常用命令
gdb默认只追踪父进程，如果要调试子进程，则在开始之前需要用如下命令：
```bash
set follow-work-mode child
start
```

如果需要同时调试父进程和子进程，则使用如下命令：
```bash
set detach-on-fork off
```
这样gdb就可以同时调试父子进程，在一个进程调试的时候另一个进程会挂起等待，如果想让父子进程同时运行，则需要用到多线程调试里提到的一个参数：
```bash
set schedule-multiple on
```

**注意**：
上面命令linux中肯定是支持的，但别的操作系统不确定
