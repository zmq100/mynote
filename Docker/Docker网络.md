### 容器访问外部网络

	# iptables -t nat -A POSTROUTING -s 172.17.0.0/16 -o docker0 -j MASQUERADE

### 外部网络访问容器

	# docker run -d -p 80:80 apache
 
> 会创建如下规则：

	# iptables -t nat -A DOCKER ! -i docker0 -p tcp -m tcp --dport 80 -j  DNAT --to-destination 172.17.0.2:80


## 进程网络修改

-b, --bridge=””   指定 Docker 使用的网桥设备，默认情况下 Docker 会自动创建和使用 docker0 网桥设备，通过此参数可以使用已经存在的设备

--bip 指定 Docker0 的 IP 和掩码，使用标准的 CIDR 形式，如 10.10.10.10/24


--dns 配置容器的 DNS，在启动 Docker 进程是添加，所有容器全部生效


## 容器网络修改

--dns 用于指定启动的容器的 DNS

--net 用于指定容器的网络通讯方式，有以下四个值

 - bridge：Docker 默认方式，网桥模式

 - none：容器没有网络栈

 - container：使用其它容器的网络栈，Docker容器会加入其它容器的 network namespace

 - host：表示容器使用 Host 的网络，没有自己独立的网络栈。容器可以完全访问 Host 的网络，不安全


## 把一张网卡改成一个网桥

```bash 

[root@localhost ~]# cd /etc/sysconfig/network-scripts/

[root@localhost network-scripts]# cp ifcfg-eth0 ifcfg-br0

[root@localhost network-scripts]# vi ifcfg-eth0

DEVICE=eth0

TYPE=Ethernet

ONBOOT=yes

BOOTPROTO=static

BRIDGE=br0

HWADDR=00:0c:29:ca:29:32

UUID=48391189-b5db-4833-b7d8-6e3fd9c3c9cd

NM_CONTROLLED=yes



[root@localhost network-scripts]# vi ifcfg-br0

DEVICE=br0

TYPE=Bridge

ONBOOT=yes

BOOTPROTO=static

IPADDR=192.168.80.10

NETMASK=255.255.255.0

GATEWAY=192.168.80.2

DNS=8.8.8.8

```

### Docker指定net为none，通过pipework把桥接网卡br0分配IP地址给docker

```bash

[root@localhost network-scripts]# yum install -y git

[root@localhost network-scripts]# git clone https://github.com/jpetazzo/pipework

[root@localhost network-scripts]# cp pipework/pipework /usr/local/bin/

[root@localhost network-scripts]# docker run -itd --net=none --name=ff centos-6-x86 bash

[root@localhost network-scripts]# pipework br0 fl 192.168.80.20/24


```
