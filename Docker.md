## 脚本安装Docker

>  更新现有 YUM 包

 - yum update

>  执行 Docker 安装脚本

 - curl -sSL https://get.docker.com/ | sh

>  启动 Docker 服务

 - systemctl start docker
 - chkconfig docker on

>  确定 Docker 安装成功

 - docker run hello-world


## YUM安装Docker

>  更新现有 YUM 包

 - yum update

>  添加 Docker 源，并安装


	# cat >/etc/yum.repos.d/docker.repo <<-EOF
	  [dockerrepo]
	  name=Docker Repository
	  baseurl=https://yum.dockerproject.org/repo/main/centos/7 enabled=1 gpgcheck=1 gpgkey=https://yum.dockerproject.org/gpg 
	  EOF
	# yum install docker-engine    
	# yum install docker


## Docker 加速设置

方法一：

	# cp /lib/systemd/system/docker.service  /etc/systemd/system/docker.service
	# chmod 777 /etc/systemd/system/docker.service 
	# vim /etc/systemd/system/docker.service
		ExecStart=/usr/bin/dockerd-current  --registry-mirror=https://5f97y8cd.mirror.aliyuncs.com \ 

	# systemctl daemon-reload
	# systemctl restart docker



方法二：

	# sudo mkdir -p /etc/docker
	# sudo tee /etc/docker/daemon.json <<-'EOF'
	  {
	  "registry-mirrors": 	 ["https://5f97y8cd.mirror.aliyuncs.com"]
	  }
	  EOF
	# sudo systemctl daemon-reload
	# sudo systemctl restart docker

[阿里云Docker官网](https://cr.console.aliyun.com/cn-hangzhou/instances/repositories)
