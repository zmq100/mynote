
# 公有仓库

### 阿里云

	# sudo docker login --username=952473625@qq.com registry.cn-hangzhou.aliyuncs.com Zz~


### 网易云

	# docker login --username=m13004994035@163.com  hub.c.163.com  c~wy


```/root/.docker/config.json
```


### 从Registry中拉取镜像

	# docker pull registry.cn-hangzhou.aliyuncs.com/zmquan/centos:[镜像版本号]


### 将镜像推送到Registry

	# docker login --username=952473625@qq.com registry.cn-hangzhou.aliyuncs.com

	# docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/zmquan/centos:[镜像版本号]

	# docker push registry.cn-hangzhou.aliyuncs.com/zmquan/centos:[镜像版本号]

----


# 仓库私有：

### 服务端设置
	# docker run -d -v /opt/registry:/var/lib/registry -p 5000:5000 --restart=always registry

	# vim /etc/docker/daemon.json
	{
    	"insecure-registries": ["192.168.80.10:5000"]
	}

	# systemctl restart docker

### 把镜像推进仓库

	# docker tag mariadb 192.168.80.10:5000/mariadb

	# docker push 192.168.80.10:5000/mariadb

### 客户机设置：

	# vim /etc/sysconfig/docker
		--insecure-registry 192.168.80.10:5000    # 增加

	# curl -XGET http://192.168.80.10:5000/v2/_catalog    #查看已有镜像

----

# 镜像分层
> 容器创建时需要指定镜像，每个镜像都由唯一的标示 ImageID，和容器的 ContainerID一样，默认128位，可以使用前 16 为缩略形式，也可以使用镜像名与版本号两部分组合唯一标示，如果省略版本号，默认使用最新版本标签(latest)

> 镜像的分层：

Docker 的镜像通过联合文件系统(union filesystem)将各层文件系统叠加在一起

> 查询镜像的分层

	# docker history 镜像名		
		--no-trunc 	显示完整的历史命令

> 镜像的导出

	# docker save 镜像ID > /home/xxx.tar 

> 镜像的导入

	# docker load < /home/xx.tar 

> 更改tag

	# docker tag [ImageId] centos:[镜像版本号]

> 把一个运行的Docker容器做成镜像

	# docker commit 当前运行的容器名 新镜像名:版本号

> 当前目录的Dockfile生成定制的image

	# docker build -t centos:latest  .  


