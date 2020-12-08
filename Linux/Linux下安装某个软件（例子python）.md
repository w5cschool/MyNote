# Linux下安装某个软件

1. 去官网下载.tgz或.gz文件

2. 将下载的文件传到Linux

3. 解压

   ```
   tar -zxvf 软件+tab补齐
   ```

4. 进入刚刚解压的文件中

5. 安装系统依赖组件：gcc openssl-devel pcre-devel zlib-devel.

   ```
   yum install gcc openssl-devel pcre-devel zlib-devel -y
   ```

6. 配置安装路径

   ```
   ./configure --prefix=/usr/local/软件名
   ```

7. 安装

   ```
   make && make install
   ```

8. 建立软连接，配置路径（相当于Windows中的PATH）

   /usr/local/python3.8/bin/python3.8，这是我们的安装路径，不同的版本，不同的安装路径需要需改

   /bin/python，可以不修改，对应以后输入python既可进入python命令行。

   ```
   ln -s /usr/local/python3.8/bin/python3.8 /bin/python3.8
   
   ```

9. 所以如果想安装多个版本的python就可以用不同的软连接来实现。

   1. ```
      ln -s /usr/local/python2.7/bin/python2.7 /bin/python2.7
      ```

      

10. 输入python3.8或python2.7验证是否安装成功

    

