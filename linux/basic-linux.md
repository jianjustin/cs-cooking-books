# 嵌入式Linux系统课程 - Linux基础

## Linux分区挂载



## Linux文件系统

1. 文件类型
   * 普通文件
   * 目录文件
   * 链接文件
   * 设备文件
2. 文件访问权限
   * 可读（r）
   * 可写（w）
   * 可执行（x）
3. 文件类型标识
   * 普通文件 -  `"-"`
   * 目录文件 -  `"d"`
   * 链接文件 -  `"l"`
   * 字符设备 -  `"c"`
   * 块设备 - `"b"`
   * `命名管道 - "p"`
   * 堆栈文件 -  `"f"`
   * 套接字 -  `"s"`
4. 文件权限访问组
   * 文件拥有者访问权限
   * 文件用户组访问权限
   * 系统其他用户访问权限
5. 文件系统类型
   * ext2
   * ext3
   * swap文件系统
   * vfat文件系统
   * NFS文件系统
   * ISO9660文件系统

## C开发环境

#### gcc编译流程

* 预处理阶段`gcc -E hello.c -o hello.i`
* 编译阶段`gcc -S hello.i -o hello.s`
* 汇编阶段`gcc -C hello.s -o hello.o`
* 链接阶段`gcc hello.o -o hello`

#### make工程管理器

* makefile配置信息

  1. 目标体，通常是目标文件或可执行文件
  2. 要创建的目标提所依赖的文件

  ```c
  hello.o: hello.c hello.h
        gcc -c hello.c -o hello.o
  ```

#### autotools

> 只需用户输入简单的目标文件，依赖文件，文件目录等就可以轻松生成makefile



## Linux系统调用

