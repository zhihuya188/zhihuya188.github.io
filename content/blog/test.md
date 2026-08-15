---
title: "git多账户管理"
aliases:
  - '/posts/test/'
date: 2019-11-02T23:40:31+08:00
description: "使用多个 GitHub 账户时的 SSH 与 remote 配置管理笔记。"
draft: false
tags:
  - Git
categories:
  - git
lastmod: 2019-11-02T12:00:00+08:00
---

> git 笔记

<!--more-->

## git

### 多账户管理

> 参考：https://io-oi.me/tech/ssh-with-multiple-github-accounts/
>
> 参考：https://www.cnblogs.com/popfisher/p/5731232.html

1. 生成github.com对应的私钥公钥
```
"""
ssh-keygen:参数
–b bits
指定要创建的密钥的位数。最小位数为 512 位。通常，2048 位足以满足安全需要。密钥大小超过该值并不会提高安全性，反而会降低速度。缺省值为 2048 位。
 
–t type
指定用于生成密钥的算法，其中 type 是 rsa、dsa 和 rsa1 中的一种。rsa1 类型仅用于 SSHv1 协议。
 
–R hostname
从 known_hosts 文件中删除属于 hostname 的所有密钥。此选项可用于删除散列主机。请参见 –H。
"""
# 帐号一
$ ssh-keygen -t rsa -b 4096 -C "1412686627@qq.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/home/archie/.ssh/id_rsa): id_rsa_yangyang188  # 起别名 

# 帐号二
$ ssh-keygen -t rsa -b 4096 -C "2633316793@qq.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/home/archie/.ssh/id_rsa): id_rsa_zhihuya188  # 起别名
```

2. 在`.ssh`目录创建`config`文本文件并完成相关配置

`vim ~/.ssh/config`编辑如下内容：

```
# 自行修改 host 和 IdentityFile
host yangyang188.github.com
    Hostname github.com
    User git
    IdentityFile /c/Users/8/.ssh/id_rsa_yangyang188

host zhihuya188.github.com
    Hostname github.com
    User git
    IdentityFile /c/Users/8/.ssh/id_rsa_zhihuya188
```
> Host的名字可以取为自己喜欢的名字，不过这个会影响git相关命令，例如：
>
> `Host mygithub` 这样定义的话，命令如下，即`git@`后面紧跟的名字改为`mygithub`
>
> `git clone git@mygithub:PopFisher/AndroidRotateAnim.git`


3. 打开`Git Bash`客户端（管理员身份运行）执行测试命令测试是否配置成功（会自动在`.ssh`目录生成`known_hosts`文件把私钥配置进去）
```
ssh -T git@[host]
```

4. 连接远程库
```
# 帐号一
~/hugo-theme-meme $ git remote set-url origin git@yangyang188.github.com:yangyang188/git_test.git

# 帐号二
 git remote set-url origin git@zhihuya188.github.com:zhihuya188/zhihuya188.github.io.git
```