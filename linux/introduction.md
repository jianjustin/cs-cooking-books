在这个模块我主要探讨在嵌入式开发中Linux的应用，学习Linux的一些基本知识

## Linux知识体系

#### 学习任务

* 磁盘管理
* 目录管理
* 用户管理
* bash-shell
* Vim编辑器
* Linux系统服务



#### 磁盘管理

1. 基本命令
   * df：列举文件系统的整体磁盘使用量
   * du：检查磁盘空间使用量
   * fdisk：磁盘分区
   * mkfs：磁盘格式化
   * fsck：磁盘检验
   * mount：磁盘挂载
   * umount：磁盘卸载



#### 文件管理

1. 文件属性
   * `d`：目录
   * `-`：文件
   * `l`：链接文档
   * `b`：可供存储的接口设备
   * `c`：串行端口设备
2. 基本命令
   * `ls`：列出目录
   * `cd`：目录切换
   * `pwd`：当前目录
   * `mkdir`：创建新目录
   * `rmkdir`：删除空目录
   * `cp`：复制文件或目录
   * `rm`：移除文件或目录
   * `mv`：移动文件或目录
3. 查看文件
   * `cat`：由第一行开始显示文件内容
   * `tac`：最后一行开始显示
   * `nl`：显示的时候，顺道输出行号
   * `more`：一页一页的显示文件内容
   * `less`：分页显示文件内容
   * `head`：只看头几行
   * `tail`：只看尾几行

4. 修改文件属性

   * #### chgrp：更改文件属组

   * #### chown：更改文件属主，也可以同时更改文件属组

   * #### chmod：更改文件9个属性



#### 用户管理

1. 基本命令
   * useradd ：添加用户
   * userdel：删除账号
   * usermod：修改用户
   * passwd：修改用户密码
   * gruopadd：添加用户组
   * groupdel：删除用户组
   * groupmod：修改用户组

## Linux嵌入式应用





* [参考学习手册](https://www.tutorialspoint.com/unix/unix-getting-started.htm)