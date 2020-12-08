# git的使用

### 一、建立GitHub与本地Git的连接



1. 安装git

2. 桌面右键打开git bash

3. 设置用户名	

   ```
   git config --global user.name 'w5cschool'
   ```

   

4. 设置用户名邮箱：

   ```
   git --global user.email '1582485954@qq.com'
   ```

   

5. 然后会让你输入密码，密码正确了就连接成功了



### 二、建立GitHub某个新仓库与本地文件夹的连接

1. 在GitHub上建立一个新的仓库
2. 打开自己要上传的文件夹，右键，选择Git bash here
3. 在GitHub新仓库上复制“or create a new repository on the command line”下的命令到刚刚打开的Git bash
4. 连接成功了，刷新GitHub会发现有了一个readme文件

注：

如果是想从GitHub上已有的仓库建立与本地文件的连接的话，就先克隆下GitHub上打仓库到本地，然后就可以直接使用了。



### 三、上传文件

1. 将文件123.txt添加到暂存区

   ```
   git add 123.txt
   ```

   

2. 将文件从暂存区提交到本地仓库

   ```
   git commit -m'提交描述'
   ```

   

3. 将文件上传到GitHub仓库

   ```
   git push
   ```

   

### 四、删除文件

1. 删除本地文件（如果远程仓库有这个文件的话可从远程仓库恢复）

   ```
   rm 123.txt
   ```

2. 删除暂存区的文件

   ```
   git rm --cached 123.txt
   ```

3. 删除远程GitHub上的文件(永远删除)

   ```
   git rm 123.txt
   git commint -m'discribe'
   git push
   ```

   



### 五、克隆项目

```
git clone https://github.com/w5cschool/MyNote
```

地址是自己要克隆的项目的地址。





### git commit和git push的区别



git作为支持分布式版本管理的工具，它管理的库（repository）分为本地库、远程库。

**git commit操作的是本地库，git push操作的是远程库。**

git commit是将本地修改过的文件提交到本地库中。
git push是将本地库中的最新信息发送给远程库。





#### 为什么要分本地commit和服务器的push呢？

因为如果本地不commit的话，修改的纪录可能会丢失。

而有些修改当前是不需要同步至服务器的，所以什么时候同步过去由用户自己选择。什么时候需要同步再push到服务器











