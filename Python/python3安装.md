## 1.安装依赖
    # yum -y install zlib-devel libffi-devel bzip2-devel openssl-devel  \
                     ncurses-devel sqlite-devel readline-devel tk-devel \
                     gdbm-devel db4-devel libpcap-devel xz-devel
    # make
    # make altinstall

## 2.做软连接
    # ln -sf /usr/local/bin/python3.7 /usr/bin/python
    # ln -s /usr/local/bin/python3.7  /usr/bin/python3

## 3.更改为yum所用的python2
    # sed -i "1s/python/python2/" /usr/bin/yum  /usr/libexec/urlgrabber-ext-down  /usr/bin/yum 
      
      #!/usr/bin/python2
