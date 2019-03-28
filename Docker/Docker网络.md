## 一、网络访问

### 容器访问外部网络

	# iptables -t nat -A POSTROUTING -s 172.17.0.0/16 -o docker0 -j MASQUERADE

### 外部网络访问容器

	# docker run -d -p 80:80 apache
 
> 会创建如下规则：

	# iptables -t nat -A DOCKER ! -i docker0 -p tcp -m tcp --dport 80 -j  DNAT --to-destination 172.17.0.2:80


## 二、进程网络修改

-b, --bridge=””   指定 Docker 使用的网桥设备，默认情况下 Docker 会自动创建和使用 docker0 网桥设备，通过此参数可以使用已经存在的设备

--bip 指定 Docker0 的 IP 和掩码，使用标准的 CIDR 形式，如 10.10.10.10/24


--dns 配置容器的 DNS，在启动 Docker 进程是添加，所有容器全部生效


## 三、容器网络修改

--dns 用于指定启动的容器的 DNS

--net 用于指定容器的网络通讯方式，有以下四个值

 - bridge：Docker 默认方式，网桥模式

 - none：容器没有网络栈

 - container：使用其它容器的网络栈，Docker容器会加入其它容器的 network namespace

 - host：表示容器使用 Host 的网络，没有自己独立的网络栈。容器可以完全访问 Host 的网络，不安全


## 四、基础命令说明

	# docker network ls 	查看当前可用的网络类型
		
	# docker network create -d 类型 网络空间名称
		类型分为：
			overlay network
			bridge network
	
## 五、独立至不同的网络名字命名空间进行隔离
	
	# docker network create -d bridge my-bridge-network
	# docker run -d --network=my-bridge-network --name test1  hub.c.163.com/public/centos:6.7-tools
	# docker run -d --name test2  hub.c.163.com/public/centos:6.7-tools
	# docker network connect my-bridge-network test2
	
## 六、Overlay 全覆盖网络（主要用于跨机器间的数据通讯问题）
	
	server1： 10.10.10.11   node1	master

	server2:  10.10.10.12   node2   slave

主机之间能够使用主机名进行通讯

Consul 是一个支持多数据中心分布式高可用的服务发现和配置共享的服务软件,由 HashiCorp 公司用 Go 语言开发, 基于 Mozilla Public License 2.0 的协议进行开源. Consul 支持健康检查,并允许 HTTP 和 DNS 协议调用 API 存储键值对

node1：

	# unzip consul_1.1.0_linux_amd64.zip 		
	# mv consul /usr/local/bin/	
	# nohup consul agent -server -bootstrap -data-dir /opt/consul -bind=10.10.10.11 &	
	# vim /lib/systemd/system/docker.service
		ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 \
				-H unix:///var/run/docker.sock \
				--cluster-store=consul://localhost:8500 \
				--cluster-advertise=eth0:2375
	
	# systemctl   daemon-reload
	# systemctl   restart   docker

node2：

	# unzip consul_1.1.0_linux_amd64.zip 		
	# mv consul /usr/local/bin/		
	# nohup consul agent -data-dir /opt/consul -bind=10.10.10.12 &
	
	# vim /lib/systemd/system/docker.service
		ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 
				-H unix:///var/run/docker.sock 
				--cluster-store=consul://localhost:8500 
				--cluster-advertise=eth0:2375
	
	# systemctl   daemon-reload
	# systemctl   restart   docker	
	# consul join node1
	# consul members
	
node1：

	# docker network create -d overlay multihos		
	# docker run --name test  --network=multihost -d hub.c.163.com/public/centos:6.7-tools
		
node2：

	# docker run --name test1  --network=multihost -d hub.c.163.com/public/centos:6.7-tools
		

登录至容器进行 Ping 测试


## 7、使用 Linux 桥接器进行主机间的通讯

```bash 

[root@localhost ~]# cd /etc/sysconfig/network-scripts/

[root@localhost network-scripts]# cp ifcfg-eth0 ifcfg-br0

[root@localhost network-scripts]# vi ifcfg-eth0

DEVICE=eth0

TYPE=Ethernet

ONBOOT=yes

BOOTPROTO=static

BRIDGE=`br0`

HWADDR=00:0c:29:ca:29:32

UUID=48391189-b5db-4833-b7d8-6e3fd9c3c9cd

NM_CONTROLLED=yes



[root@localhost network-scripts]# vi ifcfg-br0

DEVICE=`br0`

TYPE=`Bridge`

ONBOOT=yes

BOOTPROTO=static

IPADDR=192.168.80.10

NETMASK=255.255.255.0

GATEWAY=192.168.80.2

DNS=8.8.8.8

```

### 八、Docker指定net为none，通过pipework把桥接网卡br0分配IP地址给docker

```bash

[root@localhost network-scripts]# yum install -y git

[root@localhost network-scripts]# git clone https://github.com/jpetazzo/pipework

[root@localhost network-scripts]# cp pipework/pipework /usr/local/bin/

[root@localhost network-scripts]# docker run -itd --net=none --name defnet centos bash

[root@localhost network-scripts]# pipework br0 defnet 192.168.80.20/24@192.168.80.2


```
