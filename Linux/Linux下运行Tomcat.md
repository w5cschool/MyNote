## 配置Tomcat启动环境

1. 安装Java-jdk

   ​	a.获取Java所有版本下载目录

   ```
   yum -y list java*
   ```

   ​	b.选择一个版本安装

   ```
   yum install java-1.8.0-openjdk-devel.x86_64 -y
   ```

   ​	c.输入查看是否安装成功，如果能显示出版本号则表示安装成功，并且配置好了环境。

   ```
   java -version
   ```

2. 安装Apache的Tomcat

   1. 下载Apache的Tomcat
   2. 解压

   

3. 修改Apache的index.html，在index.jsp下标记"node02"，以识别是哪台服务器访问的

   ```
   cd /usr/local/apache-tomcat-9.0.40/webapps/ROOT
   vi index.jsp
   ```

4. 在Apache的bin目录下启动Tomcat

   ```
   ./startup.sh
   ```

5. 访问tomcat的index，查看是否配置成功，默认端口号为8080

   ```
   ip地址+:8080
   ```

6. 如果成功启动了，如果启动后还是无法通过ip地址访问到，则输入命令：

   1. ```
      vi /etc/sysconfig/iptables
      ```

   2. 在22 j-ACCEPT下复制一行，并将22改成80

   3. 重启防火墙即可

      ```
      service iptables restart
      ```


   或者直接关掉防火墙，如果执行不了下面的代码，参考iptables-防火墙的笔记。

   ```
systemctl stop firewalld
   ```

   