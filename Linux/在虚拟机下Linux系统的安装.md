# 在虚拟机下Linux系统的安装

## 总体脉络：

1. 下载VMwareStation并安装
2. 下载Linux系统镜像ISO
3. 新建一个虚拟机
4. 将Linux放入虚拟机
5. 开始安装
6. 给磁盘分区
7. 安装完成后打开Linux系统配置虚拟机网络





### 安装：

点击创建的虚拟机---设备---CD/DVD右键---连接---使用ISO映像文件---勾选启动时连接----选择好自己的Linux的ISO文件后--确定--开启此虚拟机---skip---basic storage Devices---yes，discard any data--
hostname(系统的名字)----设置密码---create custom layout---分区。



内存2G，分配硬盘大小100G，网络使用NAT。

这里没写的就是按推荐的来就好。

### 分区：

磁盘sda、sdb、sdc1、boot  引导程序区  ext4  200mb
2、swap   交换区  ，当内存不足时，将内存的数据写入到交换区中    两倍的内存，2048mb
3、用户区  用户使用的区     /  ext4  fill to maximun allowable size





### 配置虚拟机网络

1、找到网卡的位置
2、配置协议

在Linux中的网卡在哪呢？
cd是加载文件vi是文本编辑器

#### cd /etc/sysconfig/network-scripts/

#### ls

#### vi ifcfg-eth0

（注意这里进去后要先按一下i才能编辑）

要保证虚拟机的物理地址在多个虚拟机之间不能重复，不然可能会出现网络问题。


所以我们直接不要它，

#### 把HDADDR  UUID都删了



启用网卡：

#### 把改为：ONBOOT=yes


dhcp表示自动获取

#### 把改为：BOOTPROTO=static



编辑---虚拟网络编辑器---更改设置----NAT模式下的---NAT设置，DHCP设置查看启示ip地址和被占用的地址（不同的电脑不一样）
可知，150.0、150.255、150.1、150.2都被占用了。（不同的电脑不一样）
我的电脑是，192.168.182.0--192.168.182.255，其中0-2都被占用。
不同的虚拟机IP地址不能相同，但是掩码、网关dns那些同一台电脑都是固定的IP地址：

### IPADDR=192.168.158.66

#### 掩码：NETMASK=255.255.255.0

#### 网关：GATEWAY=192.168.182.2

DNS服务器(可以有多个)：

#### DNS1=114.114.114.114

（网关地址也可以设置为DNS地址）

#### DNS2=192.168.182.2

保存：按一下esc然后

#### :wq!

退出后再次编辑：
vi ifcfg-eth0

先点一下i
编辑完后

按下esc
：wq！回车



#### 配置后，需要重启一下网络服务。

#### service network restart

#### 测试网络是否配置成功：

ping [www.baidu.com](http://www.baidu.com/)


总结：

1、删除物理网卡地址HDADDR  +UUID

2、添加IPADDR  NETMASK  GATEWAY  DNS1  DNS2

3重启网络服务

4、ping [www.baidu.com](http://www.baidu.com/)