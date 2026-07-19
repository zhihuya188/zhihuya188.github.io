---
title: "centos7使用笔记"
date: 2019-11-08T12:16:58+08:00
description: "centos7使用笔记"
draft: false
tags:
  - Linux
  - centos
  - python
  - sqlite3
  - virtualenv
categories:
  - 专业
  - Linux
  - centos
---

> 本文主要记录了centos7一些基本使用，包括换源，创建超级用户，安装sqlite3，centos安装python3，virtualenv等

<!--more-->

<hr>

### centos命令

```
# 查看本机ip
ifconfig
```

### CentOS7 换源

1.备份

`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

2.下载新的CentOS-Base.repo 到/etc/yum.repos.d/

`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`

或者

`curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`

3.之后运行`yum makecache`生成缓存

4.其他
非阿里云ECS用户会出现 `Couldn't resolve host 'mirrors.cloud.aliyuncs.com'` 信息，不影响使用。用户也可自行修改相关配置: eg:

`sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.re`

```
# 运行以下命令生成缓存
yum clean all
yum makecache

# 更新
yum update
yum upgrade
```

### 创建超级用户

```
# 在 root 用户下运行这条命令创建一个新用户，zhangly 是用户名
root@server:~# adduser zhangly

# 为新用户设置密码
# 注意在输密码的时候不会有字符显示，不要以为键盘坏了，正常输入即可
root@server:~# passwd zhangly

# 把新创建的用户加入超级权限组
root@server:~# usermod -aG wheel zhangly

# 切换到创建的新用户
root@server:~# su - zhangly

# 切换成功，@符号前面已经是新用户名而不是 root 了
zhangly@server:$

# 更新一下系统
zhangly@server:$ sudo yum update
zhangly@server:$ sudo yum upgrade

# 安装gcc
root@server:~# yum -y install gcc
```

### 安装sqlite3

```
# 查看版本
zhangly@server:$ sqlite3 --version
# sqlite 的官方下载地址: https://sqlite.org/download.html
# 创建 src 目录并进到这个目录
zhangly@server:$ mkdir -p ~/src
zhangly@server:$ cd ~/src

# 下载 sqlite3 源码并解压安装
zhangly@server:$ wget https://sqlite.org/2019/sqlite-autoconf-3290000.tar.gz
zhangly@server:$ tar zxvf sqlite-autoconf-3290000.tar.gz
zhangly@server:$ cd sqlite-autoconf-3290000
zhangly@server:$ ./configure
zhangly@server:$ make
zhangly@server:$ sudo make install

# 重启
zhangly@server:$ sudo init 6
```


### centos安装python3

1.安装可能的依赖：

```
# 首先安装可能的依赖：
sudo yum install -y openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel
```

2.下载 Python 3.6.4 的源码并解压：

```
# 然后下载 Python 3.6.4 的源码并解压：
cd ~/src
wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz
tar -zxvf Python-3.6.4.tgz

```

3.编译安装：

```
# 最后编译安装：
cd Python-3.6.4
./configure LD_RUN_PATH=/usr/local/lib LDFLAGS="-L/usr/local/lib" CPPFLAGS="-I/usr/local/include"
make LD_RUN_PATH=/usr/local/lib
sudo make install
```

4.更新pip

```
# 更新pip  --user
pip install --upgrade pip

# 升级pip出现的错误解决
yangyang@ubuntu16-s:~$ pip
Traceback (most recent call last):
  File "/usr/bin/pip", line 9, in <module>
    from pip import main
ImportError: cannot import name main

sudo vim /usr/bin/pip

# 将原来的
from pip import main
if __name__ == '__main__':
    sys.exit(main())
# 改后的
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```

### centos安装`virtualenv`

1.安装包

```
sudo pip3 install virtualenv # 安装虚拟环境
sudo pip3 install virtualenvwrapper  # 虚拟环境扩展包
```

2.配置文件`.bashrc`

```
# 配置文件.bashrc
# 根据自己安装的路径填写 查找安装位置 whereis python3.6 | whereis virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3.6
source /usr/local/bin/virtualenvwrapper.sh
```

3.使配置文件生效

```
# 使配置文件生效
source .bashrc
```

> 常用操作

```
# 创建虚拟环境
mkvirtualenv -p python3  # 虚拟环境名称

# 例：
mkvirtualenv -p python3 py_django

# 退出虚拟环境
deactivate

# 进入虚拟环境
# 提示：workon后面有个空格，再按两次tab键。
workon 两次tab键
```


### 虚拟机centos7共享文件夹

> 在root用户下

1.重新安装缺失的组件

`yum install gcc`

`yum install kernel-devel`

2.更新刚安装的组件：

`yum update gcc -y`

`yum update kernel -y`

3.重启：

`init 6`

4.重新安装`VMware Tools`

```
tar -zxvf VMwareTools-10.2.0-7259539.tar.gz
cd vmware-tools-distrib
./vmware-install.pl
```

> 共享文件夹位置在(`/mnt/hgfs/xx`)

<hr>