## Fedora下配置网卡地址和静态ip
* 用于超算中心的网络配置，超算中心有线上网需要备案和交网费，所以只能使用静态IP入网，不能使用默认的DHCP分配IP地址；
* 以下配置方法仅在Fedora29 / Fedora27通过测试；
* 如有发现文中错误之处，欢迎指正，如果教程在现实中不work但问题被你解决，欢迎补充！

### 1. 查看网卡编号
终端输入：
> ifconfig -a

通常看到的输出如下（无关紧要的输出信息已省略）：
```
eno1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet xxx.xxx.xxx.xxx  netmask xxx.xxx.xxx.xxx  broadcast xxx.xxx.xxx.xxx
        inet6 fe80::6fbc:b319:d89:571c  prefixlen 64  scopeid 0x20<link>
        inet6 2001:250:3002:4900:54b8:a929:e23d:f444  prefixlen 64  scopeid 0x0<global>
        ether xx:xx:xx:xx:xx:xx  txqueuelen 1000  (Ethernet)
        ...
		...

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        ...
		...
```

* 其中**lo**是指本机内部环路；
* **wlxxx**一般是无线网卡编号；
* **eno1**就是我们要查询的有线网卡编号（可能你的是enp0 / eth0）。

### 2. 进入目录
终端输入：
> cd /etc/sysconfig/network-scripts/

### 3. 配置文件
终端输入：
> sudo vi ifcfg-[你的有线网卡编号]

例如：
> sudo vi ifcfg-eno1

修改配置如下：
* 只有**\#**号之间的位置为修改过的，其他原本就在；
* 其中要注意，配置MAC地址要用**MACADDR**属性！网上99%的教程都教你用**HWADDR**属性，但这样配置会出问题，笔者不清楚是Fedora版本的问题还是其他。
```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
############################
BOOTPROTO=static								# 原本为dhcp，改为static或none
MACADDR=xx : xx : xx : xx : xx : xx		# MAC地址
IPADDR=172.18.xxx.xxx						# IP地址
NETMASK=255.255.254.0					# 子网掩码
GATEWAY=172.xxx.xxx.xxx					# 网关
DNS1=10.xxx.xxx.xxx							# DNS
DNS2=10.xxx.xxx.xxx							# DNS
# NM_CONTROLLED=yes/no
############################
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=eno1
UUID=14136b1e-008b-3c0d-ab29-9bde4b1c23d5
ONBOOT=yes
AUTOCONNECT_PRIORITY=-999
DEVICE=eno1
```

* NM_CONTROLLED属性中，NM表示Network Manager；
* 如果属性为yes，则NM会在/etc/下创建resolv.conf来记录DNS，默认为yes。

### 4. 重启网络服务
终端输入：
> systemctl restart NetworkManager.service

或者
> systemctl restart network.service

