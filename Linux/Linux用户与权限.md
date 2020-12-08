# Linux用户与权限



9位：333--->属主，属组，其他

### 创建普通用户：

```
useradd xiaoxiao
passwd xiaoxiao
```



### 彻底删除用户以及相关文件：

```
userdel xiaoxiao
rm -rf xiaoxiao
cd /var/spool/mail/
rm -rf xiaoxiao
```

### 创建共享文件夹

1. 创建一个文件夹

   ```
   mkdir share
   ```

   

2. 创建一个组

   ```
   groupadd share
   ```

   

3. 将用户xiaoxiao添加到share组中

   ```
   usermod -G share xiaoxiao
   ```

4. 查看是否成功加入

   ```
   id xiaoxiao
   ```

5. 将文件夹的属组改为share

   ```
   chown root:share share/
   ```

6. 给属组加上权限

   ```
   chmod g+w share/
   ```

7. chmod o-r share其他用户没有权限

   ```
   chmod o-r share
   chmod o-x share
   ```

8. 重启





### 切换用户

```
su xiaoxiao
```

rwx	2^2	+	2^1	+	2^0 = 7





授权：

```
chmod 764 xioaxiao
```

或者

```
chmod o+w xiaoxiao
chmod u+w xiaoxiao
chmod +w xiaoxiao
```





修改

```
chown :share xiaoxiao
```



