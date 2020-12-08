# 给Windows的虚拟机下的Linux配置Nginx



## 一、安装Nginx

1. 在虚拟机安装Linux系统，配置好

2. 在Windows安装xshell和xftp用于方便与Linux交互

3.  将xshell与Linux建立连接

4. 下载Tengin（http://tengine.taobao.org/）

5. 打开xshell，用xftp将刚刚下载到的Windows的tengin复制到Linux的~下，也就是/root下

6. 在Linux下将tengin解压缩：

   ```
   tar -zxvf tengin+tab(自动补全文件名)
   ```

   

7. 进入刚刚解压的文件中

   ```
   cd teng+tab
   ```

   

8. 安装系统依赖组件：gcc openssl-devel pcre-devel zlib-devel.

   ```
   yum install gcc openssl-devel pcre-devel zlib-devel -y
   ```

9. 配置该文件

   1. 配置安装路径：

      ```
      ./configure --prefix=/usr/local/tengine
      ```

      (prefix后面跟的是要安装的路径)

10. 安装Nginx：

    ```
    make && make install
    ```

11. 安装完成

## 二、启动Nginx

### 手动启动Nginx

1. 进入tengin文件目录，

   ```
   cd tengin
   ll
   ```

   可以看到有4个目录：

   1. conf	放配置文件
   2. html     放静态文件
   3. logs     放日志
   4. sbin      放可执行命令

2. 进入sbin

   ```
   cd sbin
   ```

3. 启动Nginx

   ```
   ./nginx
   ```

4. 这是Nginx就启动了，就可以在网页访问通过这台虚拟机的ip地址访问了。

5. 如果启动后还是无法通过ip地址访问到，则：

   1. 输入命令

      ```
      vi /etc/sysconfig/iptables
      ```

   2. 在22 j -ACCEPT下复制一行，并将22改成80

   3. 重启防火墙即可：

      ```
      service iptables restart
      ```

      

   

6. 将Nginx停下来

   1. 查看Nginx的相关进程

      ```
      ps -ef | grep nginx
      ```

   2. 关停所有有关Nginx的进程(通过刚刚查出来的有关Nginx的线程号来确定number)

   ```
   kill -9 number
   ```



### shell脚本启动Nginx

1. 在/etc/init.d/下建立一个nginx文件

2. 用vi打开nginx文件

3. 将shell脚本的内容粘贴进nginx文件中,如果nginx安装的目录不是在usr/local下，则需要修改一下nginx的变量值

   ```shell
   
   #!/bin/sh
   #
   # nginx - this script starts and stops the nginx daemon
   #
   # chkconfig:   - 85 15 
   # description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
   #               proxy and IMAP/POP3 proxy server
   # processname: nginx
   # config:      /etc/nginx/nginx.conf
   # config:      /etc/sysconfig/nginx
   # pidfile:     /var/run/nginx.pid
    
   # Source function library.
   . /etc/rc.d/init.d/functions
    
   # Source networking configuration.
   . /etc/sysconfig/network
    
   # Check that networking is up.
   [ "$NETWORKING" = "no" ] && exit 0
    
   nginx="/usr/local/tengine/sbin/nginx"
   prog=$(basename $nginx)
    
   NGINX_CONF_FILE="/usr/local/tengine/conf/nginx.conf"
    
   [ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
    
   lockfile=/var/lock/subsys/nginx
    
   make_dirs() {
      # make required directories
      user=`nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
      options=`$nginx -V 2>&1 | grep 'configure arguments:'`
      for opt in $options; do
          if [ `echo $opt | grep '.*-temp-path'` ]; then
              value=`echo $opt | cut -d "=" -f 2`
              if [ ! -d "$value" ]; then
                  # echo "creating" $value
                  mkdir -p $value && chown -R $user $value
              fi
          fi
      done
   }
    
   start() {
       [ -x $nginx ] || exit 5
       [ -f $NGINX_CONF_FILE ] || exit 6
       make_dirs
       echo -n $"Starting $prog: "
       daemon $nginx -c $NGINX_CONF_FILE
       retval=$?
       echo
       [ $retval -eq 0 ] && touch $lockfile
       return $retval
   }
    
   stop() {
       echo -n $"Stopping $prog: "
       killproc $prog -QUIT
       retval=$?
       echo
       [ $retval -eq 0 ] && rm -f $lockfile
       return $retval
   }
    
   restart() {
       configtest || return $?
       stop
       sleep 1
       start
   }
    
   reload() {
       configtest || return $?
       echo -n $"Reloading $prog: "
       killproc $nginx -HUP
       RETVAL=$?
       echo
   }
    
   force_reload() {
       restart
   }
    
   configtest() {
     $nginx -t -c $NGINX_CONF_FILE
   }
    
   rh_status() {
       status $prog
   }
    
   rh_status_q() {
       rh_status >/dev/null 2>&1
   }
    
   case "$1" in
       start)
           rh_status_q && exit 0
           $1
           ;;
       stop)
           rh_status_q || exit 0
           $1
           ;;
       restart|configtest)
           $1
           ;;
       reload)
           rh_status_q || exit 7
           $1
           ;;
       force-reload)
           force_reload
           ;;
       status)
           rh_status
           ;;
       condrestart|try-restart)
           rh_status_q || exit 0
               ;;
       *)
           echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
           exit 2
   esac
   ```

4. 给nginx添加权限

   ```
   chmod 777 ./nginx
   ```

5. 启动Nginx

   ```
   service nginx start
   ```

6. 如果启动后还是无法通过ip地址访问到，则输入命令：

   1. ```
      vi /etc/sysconfig/iptables
      ```

   2. 在22 j-ACCEPT下复制一行，并将22改成80

   3. 重启防火墙即可

      ```
      service iptables restart
      ```

      

7. 查看nginx启动后的进程

   ```
   ps -ef | grep nginx
   ```

8. 停止nginx

   ```
   service nginx stop
   ```





注意事项：

Nginx不能让root用户去安装。





### 配置nginx.conf

1. ```
   vi /usr/local/tengin/conf/nginx.conf
   ```

2. worker_processes:可以开多少个进程给Nginx，默认是1，一般我们会将其设置为CPU的内核数一样。

3. worker_connects:一个worker_processes可以承受的线程数。官方说nginx可以承受5万的并发总量。

4. Linux服务器能打开多少个文件限制了进程的性能。

5. 查看Linux内核能打开的句柄数：

   ```
   cat /proc/sys/fs/file.max
   ```

6. 所以worker_connects*worker_processes<=5万<=句柄数

7. 查看Linux的允许打开的最大句柄数：

   ```
   1. cat /proc/sys/fs/file.max
   
   ```

8. include:mime.type文件名对应的类型，.mp3,.pgn之类的。服务器给也没定义的文件类型。

9. default_type:浏览器默认能识别的文件类型，如果打开的文件类型没有在里面，则直接弹出下载框给用户。

10. sendfile

    ![Image](C:\Users\TIANYA~1\AppData\Local\Temp\chrome_drag17804_16342\Image.png)

11. http{servier{表示一个虚拟主机}}

12. 默认访问的index站点

    ```
    localtion / {
    root	myHtml;
    index	index.html	index.htm	a.html;
    }
    ```

    /:以/开头的都进入location里面去匹配资源，注意/前后都有空格，
    
13. autoindex on:主页显示可访问目录。alias==root.一个server只能有一个root，但可以有多个alias

```

localtion / {
alias myHtml;
indexauto on;

}
```



# 访问Nginx下html的路线

请求网页--->进入localtion / 下--->寻找root--->root 对应myHtml--->去myHtml寻找网页地址---->找到了就返回给网页







