# Linux的软件安装



1、基于软件源码

2、基于编辑好的安装包





查找安装的jdk

```
rpm -qa | grep jdk
```

软件包包含依赖检查，但还需要认为解决。

卸载软件

rpm -e name







rpm安装：



rpm -ivh --prefix /usr/local







配置修改yum源：



1、修改配置文件

2、yum clean all

3、yum makecache







cd /etc/yum.reposd/



mv CentOS-Base.repo bak/





mirror.alis.com





















































































