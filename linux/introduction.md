在这个模块我主要探讨在嵌入式开发中Linux的应用，学习Linux的一些基本知识

## Linux知识体系

#### 学习任务

* 磁盘管理
* 目录管理
* 用户管理
* bash-shell
* Vim编辑器
* Linux系统服务

## 嵌入式Linux课程说明

* [Linux基础](./basic-linux.md)
* [Linux基础命令](./basic-command.md)
* [嵌入式Linux系统](./basic-embeded-linux.md)

## Linux嵌入式应用



#### Linux启动过程

1. BIOS自检
   * 上电自检POST
   * 执行一段小程序用来枚举本地设备对其初始化
2. 内核引导
3. 执行init程序进行系统初始化
4. init启动mingetty，打开shell终端供用户登录进入系统

* [参考学习手册](https://www.tutorialspoint.com/unix/unix-getting-started.htm)