# Nginx实现负载均衡



用户请求--->nginx----转发到---->tomcats------>根据算法分配到各个tomcat----->将结果返回给nginx------>用户



## 一、配置Tomcat启动环境

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
   
      

## 二、将nginx设为代理服务器，转发到tomcats下的服务器

1. 将nginx设为代理服务器，转到tomcats下的服务器

   1. 进入的nginx.conf

      ```
      cd /usr/local/tengin/conf
      vi nginx.conf
      ```

   2. 在location / {}中

      ```
      location / {
      	proxy_pass http://tomcats;
      }
      ```

   3. 在service同级下加入（upstream是关键字)

   ```
   upstream tomcats{
   	server 192.168.182.22:8080;
   }
   ```

2. 启动nginx

   ```
   service nginx start
   ```

3. 访问nginx的index，ip+端口号

   ```
   192.168.182.22:80
   ```

4. 如果成功跳转到tomcat下的index.jsp页面，则配置成功



## 三、启动多台tomcat服务器实现负载均衡

1. 启动另一台虚拟机

2. 配置好tomcat

   1. 进入tomcat的service.xml

      ```
      cd /usr/local/apache/conf/
      vi service.xml
      ```

      

   2. 将tomcat的Connector port="8080" protocol="HTTP/1.1端口号改为8081

      ```
      　server.xml文件中有三个端口设置：
      
      　　<Server port="8005" shutdown="SHUTDOWN"> ：关闭时使用
      
      　　<Connector port="8081" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" /> ： 一般应用使用，网页访问的端口
      
      　　<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />：为AJP端口，即容器使用，如 APACHE能通过AJP协议访问Tomcat的8009端口
      ```

      

   3. 将tomcat的index.jsp表示为node03

3. 在刚刚的nginx.conf下的upstream里加入这个tomcat服务器，与server同级加入一下代码。

   ```
   upstream tomcats{
   	server 192.168.182.22:8080;
   	server 192.168.182.44:8081;
   }
   ```

   

4. 重新启动nginx

5. 访问nginx，会发现node02和node03轮询被访问。说明两台服务器的负载均衡配置成功。

6. 给服务器添加权重，这样就不是按照轮询的方式配置负载了（1:1），而是给了第一个server配置了更高的权重，这样分发到第一个server的次数就会更多。

   ```
   upstream tomcats{
   	server 192.168.182.22:8080 weight=2;
   	server 192.168.182.44:8081 weight=1;
   }
   ```

   

#### 健康检查模块

配置一个status的location

```
location /status {
            check_status;

        }
```

在upstream配置如下

```
check interval=3000 rise=2 fall=5 timeout=1000 type=http;
check_http_send "HEAD / HTTP/1.0\r\n\r\n";
check_http_expect_alive http_2xx http_3xx;
```



访问ip+/status







