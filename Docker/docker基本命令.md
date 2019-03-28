
## 容器的基本命令

> 守护进程的系统资源设置

	docker info	

> Docker 仓库的查询
	
	docker search

> Docker 仓库的下载
		
	docker pull

> Docker 镜像的查询
	
	docker images
		
>Docker	镜像的删除

	docker rmi
		
> 容器的查询

	docker ps
		
> 容器的创建启动

	docker run	

> 容器启动停止
 
	docker start/stop	


> 查看

	docker ps --no-trunc	


> 容器的自动启动

	docker run --restart=always	  

> 通过容器别名启动/停止

	docker start/stop MywordPress 

> 查看容器所有基本信息

	docker inspect MywordPress   

> 查看容器日志

	docker logs MywordPress  

> 查看容器所占用的系统资源

	docker stats MywordPress  

> 容器执行命令

	docker exec 容器名 容器内执行的命令  

> 登入容器的bash

	docker exec -it 容器名 /bin/bash  

> 容器挂载目录

选项： 

	-v   [host-dir]:[container-dir]:[rw|ro]
	--volumes-from=””


## 初步体验 Docker - WordPress

WordPress 运行环境需要如下软件的支持：

 - PHP 5.6 或更新软件

 - MySQL 5.6 或 更新版本

 - Apache 和 mod_rewrite 模块

```bash 

	# docker run --name db --env MYSQL_ROOT_PASSWORD=example -d mariadb

	# docker run --name MyWordPress --link db:mysql -p 8080:80 -d wordpress

```


## 初步体验 Docker - GitLab

GitLab 运行环境需要如下软件的支持：

 - postgresql

 - redis 缓存服务

 - gitlab 服务

启动 postgresql：

	docker run --name gitlab-postgresql -d \
		--env 'DB_NAME=gitlabhq_production' \
		--env 'DB_USER=gitlab' \
		--env 'DB_PASS=password' \
		sameersbn/postgresql:9.4-12

启动 Redis：

	# docker run --name gitlab-redis -d sameersbn/redis:latest

启动 gitlab：

	docker run --name gitlab -d \
	--link gitlab-postgresql:postgresql \
	--link gitlab-redis:redisio \
	--publish 10022:22 \
	--publish 10080:80 \
	--env 'GITLAB_PORT=10080' \
	--env 'GITLAB_SSH_PORT=10022' \
	--env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
	sameersbn/gitlab:8.4.4
