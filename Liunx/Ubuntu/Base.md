## 一、更改网卡配置信息

	# vim /etc/network/interfaces

	# interfaces(5) file used by ifup(8) and ifdown(8)
	auto lo
	iface lo inet loopback
	
	auto ens33
	iface ens33 inet static
	address 192.168.80.80
	netmask 255.255.255.0
	gateway 192.168.80.2
	dns-nameservers 119.29.29.29

> 重起网卡

	# /etc/init.d/networking restart 
	# systemctl restart networking
	# systemctl restart network-manger

## 二、更改软件源：163

	# vim /etc/apt/source.list

	deb http://mirrors.163.com/ubuntu/ wily main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ wily-security main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ wily-updates main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ wily-proposed main restricted universe multiverse
	deb http://mirrors.163.com/ubuntu/ wily-backports main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ wily main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ wily-security main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ wily-updates main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ wily-proposed main restricted universe multiverse
	deb-src http://mirrors.163.com/ubuntu/ wily-backports main restricted universe multiverse

> 更新软件索引和更新软件

	# apt update
	# apt upgrade

## 三、apt进程被占用

> apt在更新软件时会遇到apt进程被占用时，可用以下方法杀死进程。

	# rm -f /var/lib/dpkg/lock 
	# rm -f /var/lib/dpkg/lock

	# ps -ef | grep apt
	# kill $PID

## 四、配置Ubuntu的JDK和Maven环境

	# vim /etc/profile

	  #JDK1.8
	  export JAVA_HOME=/usr/local/jdk/jdk1.8.0_191
	  export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
	  export CLASSPATH=$JAVA_HOME/lib:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	
	  #MAVEN3.6    
	  export M2_HOME=/usr/local/jdk/apache-maven-3.6.0
	  export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$M2_HOME/bin

> 使配置文件生效
	
	# source /etc/profile

## 五、ubuntu桌面快捷方式

	# vim /usr/share/applications/sts4.desktop

	  [Desktop Entry]
	  Version=1.0
	  Encoding=UTF-8
	  Name=Spring Tool Suite 4
	  Comment=Spring Tool Suite 4
	  Exec="/usr/local/sts-4.0.1/SpringToolSuite4    #根据软件的具体执行路径修改
	  Icon=/usr/local/sts-4.0.1/icon.xpm             #根据软件的具体执行路径修改
	  Terminal=false                                 #软件打开时是否启动终端
	  StartupNotify=false
	  Type=Application
	  Categories=Application;IDE
	  StartupWMClass=Spring Tool Suite 4
	  #在终端中输入xprop WM_CLASS   回车后点击要查询的软件窗体可以获得
