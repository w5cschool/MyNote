自制ssl证书







1. 下载OpenSSL

2. 进入OpenSSL下面的bin目录打开cmd

3. 进入

   ```
   openssl.exe
   ```

4. 生成私钥key

   ```
   genrsa
   ```

5. ```
   key--私钥 = 明文，自己生成的
   csr--公钥--由私钥生成
   crt--证书--公钥+签名
   ```

6. 由私钥生成公钥

   ```
   openssl req -new -key c:/dev/my.key -out c:/dev/my.csr
   openssl req -new -key server.key -out server.csr
   ```

7. 查看证书信息

   ```
   req -text -in c:/dev/my.csr -noout
   ```



























### Linux生成

1. 可以直接进入openssl（Linux一般都自带了openssl）

2. 生成私钥，并给该私钥加个密码（以后有人想用这个私钥就要输入密码才能使用了）

   ```
   openssl genrsa -des3 .out server server.key 102
   ```

3. 由私钥生成公钥，填入相关信息。

   ```
   openssl req -new -key server.key -out server.csr
   
   ```

4. 给自己签名，生成证书crt

   ```
    openssl x509 -req -days 365 -in server.csr -signkey server.key.unsecure -out server.crt
   
   ```

5. 将证书拷贝到nginx下

   ```
   mv server.crt /usr/local/tengine
   ```

   

6. 将nginx.conf下的HTTPS server原本注释的代码解开。

7. 将crt和key的地址填入相关位置

8. 重启nginx

   ```
   service nginx stop
   service nginx start
   ```

9. 输入key的密码

10. 就可以访问加上https的自己的网址了。







总结：先生成私钥，再由私钥生成公钥，再给自己签名（生成证书），配合nginx.conf（将crt和key的地址填入HTTPS server中)。













