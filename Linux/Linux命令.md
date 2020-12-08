# Linux命令







### 需要下载两个软件工具：Xshell、XFTP。



在Xshell中与虚拟机建立联系：

##### ssh root@192.168.150.111

输入密码



在虚拟中查看ip地址：

ifconfig     



## 外部命令和内部命令



内部命令：shell自带的命令

外部命令：不是shell自带的命令，由用户安装的



### 什么是shell？



bash shell是一个程序，软件。用于客户与操作系统打交道的。

1、登录后

2、cd/etc/

​	1、根据空格切割字符串，把第一个位置认为是命令，其他的位置认为是命令的参数

​	2、判断命令是内部的还是外部的

​	3、如果是外部的话，就去找这个命令的可执行文件

3、到Linux执行命令





### 通过type+命令查看命令类型

如

type cd

type ifconfig

cd is a shell ...		表示内部 命令

ifconfig is hashed(/.)	表示外部命令（带有路径）



### 使用cat查看文件内容

cat 123.txt

### file查看文件类型：

file ifconfig



### 使用whereis 查看可执行文件的路径

whereis ifconfig



### 使用外部命令效率是不是很慢？

不是，他之会区PATH寻找



### 打印字符串：echo "123"





### 打印变量:echo $PATH



### 如何使用一个陌生的命令

查看ifconfig怎么用：

1、man ifconfig

2、ifconfig help 

一般来说查看外部命令用man，内部命令用help。

如果提示没有man这个命令：

则安装

yum install man

















