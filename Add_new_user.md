## Linux下创建新用户
* 用于配置服务器或共享GPU资源；
* 如果发现文中错误之处，欢迎指正，如果教程在现实中不work但问题被你解决，欢迎补充！

### 1. 创建新用户
终端输入：
```
sudo adduser USER_NAME -home HOME_PATH
```
例如：
```
sudo adduser tmp -home /home/share/tmp
```

注意：
1.  这里有一个很有趣的地方，HOME_PATH用以下两种方式有一点区别:
> /home/share/tmp 

和
> /home/share/tmp/
	
2. 在ssh连接后，执行命令：
```
cd ~
```
3. 前者会显示：
```
[xxx@xxx ~]$ 
```
4. 后者会显示：
```
[xxx@xxx /home/share/tmp]$ 
```
5. 除此之外没有发现其他影响。

### 2. 根据提示输入密码等信息
* 略

### 3. 开放ssh登陆
* 略

### 4. 踩过的坑
1. 权限问题，如出现permission denied错误，检查文件权限。
