# Nginx实现用户认证访问：



1. 安装httpd tools用于生成用户字典：

   ```
   yum install httpd
   ```

2. 生成用户密码字典：

   ```
   htpasswd -c -d /usr/local/tengine/conf/users tian
   ```

3. 在nginx.conf文件下，在需要权限认证的location下加入：

   ```
   #权限认证，密码藏在users中，auth_basic_user_file后面跟的是刚刚生成的用户密码存放的位置
   auth_basic  "closed site";
   auth_basic_user_file users;
   
   ```

4. 重启服务

   ```
   service nginx reload
   ```

5. 访问刚刚加了权限认证的页面，如果成功了就会有输入框提示输入密码。（按上面的代码，用户为tian）

6. 注意，输入一次后网页可能会记住密码，以后再次访问的时候可能就不需要输入密码了，所以我们可以打开无痕窗口来测试我们的代码。

