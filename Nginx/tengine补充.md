

同步时间：

service ntpd status

开机启动nginx

```
chkconfig --list
chkconfig --add nginx
chkconfig nginx on
```



所有location中只能有一个root，如果想要多个路径去找文件，可以用alias。autoindex是指显示列表页。

```
location /ww1 {
	alias myHtml2;
	autoindex on;
}
```

### 查看当前应用所占用端口：

netstat  -nlp | grep 8080

//8080是系统启动访问的端口， 由此可得到9578 是java运行的端口，



kill -9 9578





1. maven install
2. 传输到Linux
3. java -jar Springboot+tab
4. 















