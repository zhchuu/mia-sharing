## Fedora下使用Systemd启动、管理守护进程
* 该方法用于开机启动某些后台程序，同时可以方便管理守护进程。如果你有什么命令希望开机自动执行、或者希望程序执行为守护进程，**Systemd**是很好的解决方案，值得学习；
* **Systemd**为系统的启动和管理提供了一套完整的解决方案，字母**d**是守护进程（daemon）的缩写;
* 以下方法仅在Fedora29 / 27通过测试；
* 如有发现文中错误之处，欢迎指正，如果教程在现实中不work但问题被你解决，欢迎补充！

### 1.创建systemd启动脚本
在目录`/etc/systemd/system/`下创建`[Your-Service].service`，内容如下：
```
[Unit]
After=network.service

[Service]
ExecStart=/path/to/bash/[xxx].sh
Type=forking

[Install]
WantedBy=default.target
```
* After：该服务什么时候服务执行，这里指的是在network这个服务执行完毕后，再执行本服务；
* ExecStart：要执行的bash脚本的路径；
* Type：如果需要服务一直在后台运行，则`Type=forking`，进程启动后会执行`fork()`，而父进程会`exit()`；如果执行完毕就结束，则`Type=simple`，进程执行完毕后`exit()`。

### 2.设置文件权限
修改bash脚本权限：
> chmod 744 /path/to/bash/[xxx].sh

修改systemd启动脚本权限：
> chmod 664 /etc/systemd/system/[Your-Service].service

### 3.创建服务
执行命令：
> systemctl daemon-reload
systemctl enable [Your-Service].service

会显示：
```
Created symlink from /etc/systemd/system/default.target.wants/[Your-Service].service to /etc/systemd/system/[Your-Service].service.
```

如果要撤销这项服务，则执行：
> systemctl disable [Your-Service].service

会显示：
```
Removed /etc/systemd/system/default.target.wants/[Your-Service].service.
```

### 4.查看服务状态及管理
* 启动：
> systemctl start [Your-Service].service

* 停止：
> systemctl stop [Your-Service].service

* 重启：
> systemctl restart [Your-Service].service

* 查看状态：
> systemctl status [Your-Service].service
