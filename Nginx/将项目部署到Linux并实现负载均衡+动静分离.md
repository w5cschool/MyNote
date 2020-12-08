# Linux部署项目---负载均衡+动静分离





### 一、将项目打包成jar包部署到Linux

1. 修改项目的配置文件的数据库地址，可以将localhost修改为项目的数据存放的电脑的ip地址

2. 给该项目在登录页标识为是Linux运行的项目，比如加个<h1>linux22</h1>

3. 修改数据库的权限--administration--users and privileges---limit to hosts matching :改为%

4. maven clean

5. maven install

6. 传输到Linux

7. 运行jar项目：

   ```
   1. java -jar Springboot+tab
   ```

8. 子Windows上也运行该项目，并先将刚刚的标识改为windows



### 二、负载均衡

1. 在Linux的上，配置nginx的nginx.conf。

   两台服务器：一个运行的jar包项目的Linux的IP地址及端口号，一个是Windows的运行的项目的ip地址及端口号，权重为1:1

   ```
   
    upstream tomcats {
                   server 192.168.1.102:8080 weight=1;
                   server 192.168.182.22:8080 weight=1;
           }
   
       server {
           listen       80;
           server_name  localhost;
   
           #将nginx设为代理，将用户访问nginx的请求转到tomcats下的服务器
           location / {
                   proxy_pass http://tomcats;
           }
   
   ```

2. 重启记载nginx

   ```
   service nginx reload
   ```

3. 现在就可以访问Linux的nginx的IP地址，不停的刷新页面就会发现页面的标识会在Linux和Windows切换。说明项目部署到两个服务器成功，并实心负载均衡成功。

4. 待解决的问题：

   1. 两台服务器之间的session共享
   2. 优化：动静分离，将静态页面放到nginx上，动态页面在tomcat上。



### 三、动静分离

1. 将项目的static下的文件（js,css,jpn,png...）拷贝到nginx的my_static（自己建的）

2. 将项目的静态文件删除

3. 将项目打包成jar到Linux

4. 在Linux和Windows都运行该项目

5. 在nginx的nginx.conf文件下加一个location，表示匹配以gif,jpg,css...结尾的所有文件（即静态文件）

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|html|htm|css|js)$ {
               alias  var/data/www1;
              
           }

6. 重启nginx

   ```
   service nginx reload
   ```

   

7. 会发现网页的css等依旧生效

8. 如果图片显示不了，可能是因为链接最前面有/

9. 但是登陆不进去，因为没有写session共享，所以会一直登陆不进去。

10. 前后不要有/





















