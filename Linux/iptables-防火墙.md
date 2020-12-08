# Linux无iptables的解决方法



## 检查是否安装了iptables

service iptables status

### 安装iptables

yum install -y iptables

### 升级iptables

yum update iptables

### 安装iptables-services

yum install iptables-services

##### iptables-services 和 iptables 是不一样的

安装了 services才有/etc/sysconfig/iptables

禁用/停止自带的firewalld服务

### 停止firewalld服务

systemctl stop firewalld

### 禁用firewalld服务

systemctl mask firewalld



