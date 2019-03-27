## 一、安装依赖

	# yum install -y curl-devel expat-devel gettext-devel  gcc \
				     openssl-devel zlib-devel  perl-ExtUtils-MakeMaker

 

## 二、卸载原版
	# yum remove git

 

## 三、解压安装

	# cd /usr/src
	# wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
	# tar xzf git-2.9.5.tar.gz
	# cd git-2.9.5
	# make prefix=/usr/local/git all
	# make prefix=/usr/local/git install
	
	# ln -s /usr/local/git/bin  /usr/local/bin

 

## 四、全局配置(和三个配置文件)

	$ git config --global user.name "zmq"
	$ git config --global user.email "zmq@zmq100.cn"
	$ git config --global credential.helper store      >>  .git-credentials
	$ git config --system --unset credential.helper
	$ git config --global http.emptyAuth true

 

> **.git-credentials**

　　http://user:password@ip


>  **.gitconfig**

　　[user]

　　　　name = zmq

　　　　email = zmq@zmq100.cn

　　[http]

　　　　emptyAuth = true

　　[credential]

　　　　helper = storemanager


> **.minttyrc**

　　BoldAsFont=-1

　　Language=zh_CN

　　RightClickAction=paste


## 五、创建提交版本库

> 把目录变成repository版本库，生成.git目录（stage、master、HEAD)

	$ git init　　

> 从工作区添加到暂存区（stage）

	$ git add file.txt

> 从暂存区（stage）一次性全部提交到当前分支（master）

	$ git commit -m "comment"　　　    

> 查看状态

	$ git status

> 查看修改内容

	$ git diff file.txt 　　　　　  　 

 

## 六、版本回退

> 查看历史纪录
	$ git log							

> 简短查看历史纪录
	
	$ git log –pretty=oneline			

> 查看回退的版本号
	
	$ git reflog						

> 回退到上一个版本^
	
	$ git reset --hard HEAD^ 			

> 回退到上一个版本
	
	$ git reset --hard HEAD~1			

> 丢弃工作区的的修改/恢复（先回退到暂存区---没有在回到版本库）
	
	$ git checkout -- file.txt			

 

## 七、远程仓库

> Clone 远程仓库

	$ git clone https://github.com/zmquan/test.git

> 拉取远程仓库

	$ git pull

> 将当前分支推送到origin主机的master分支

	$ git push origin master


> 关联远程仓库分支跟本地的分支

	$ git branch --set-upstream-to origin master 或者 git branch -u origin master


## 八、总结创建与合并分支命令如下

> 查看本地分支

	$ git branch  
                    
> 查看远程分支

	$ git branch -a                   

>创建分支

	$ git branch name                 

>切换分支

	$ git checkout name               

>创建+切换分支

	$ git checkout –b name            

>合并某分支到当前分支

	$ git merge name                  

>删除本地分支

	$ git branch –d name              

>删除远程分支

	$ git push origin -–delete name   
