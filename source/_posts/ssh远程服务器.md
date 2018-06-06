---
title: ssh远程连接服务器
# date: 2018-06-05 23:56:24
tags: [ssh]
---

# ssh远程连接服务器

原理很简单： 本地生成密钥（公钥和私钥）， 将公钥配置到服务器， 这样就可以在安全的登录服务器了。也可以先在服务器生成密钥， 这个就没有尝试了， 原理应该都是一样的。

1. 本地生成密钥
```bash
ssh-keygen -t rsa -P ''   # -P表示密码， 也可以不加-P
```
`~/.ssh`文件夹下会生成密钥

<!--more-->

2. 将公钥复制到服务器
```bash
scp ~/.ssh/id_rsa.pub root@ip:~/.ssh/authorized_keys  # 将公钥复制到服务器（root和ip替换成服务器用户名和ip）
```
或
```bash
scp ~/.ssh/id_rsa.pub  root@ip:~/  # 将公钥复制到服务器根目录
ssh root@ip      # 登录服务器
mkdir ~/.ssh     # 创建.ssh目录， 如果存在可以忽略
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys   # 将公钥id_rsa.pub追加到authorized_keys文件
```
mac（我使用的这种方式）
```bash
brew install ssh-capy-id
ssh-copy-id root@ip   # root和ip替换成服务器用户名和ip
cat ~/.ssh/id_rsa.pub | ssh root@ip "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"   # 将公钥复制到服务器对应目录下的authorized_keys下
```
**authorized_keys的权限要是600**
```bash
chmod 600 ~/.ssh/authorized_keys   # 只有root用户才有读写权限
```

3. SSH配置文件(仅远程连接服务器，这一步不需要)
```bash
vim /etc/ssh/sshd_config
```
取消注释
```bash
# RSAAuthentication yes
# PubkeyAuthentication yes
# AuthorizedKeysFile .ssh/authorized_keys
```
```bash
PasswordAuthentication no
```
4. 重启SSH服务
```bash
service sshd restart
```
重启时出现
`Job for sshd.service failed because the control process exited with error code. See "systemctl status sshd.service" and "journalctl -xe" for details.`
解决办法
使用 `sudo /usr/sbin/sshd -T` 找到问题出在哪里(我这里是拼写错误。。。搞了一晚上)

参考资料：
[在Mac上远程登录VPS](https://www.jianshu.com/p/460b4ce4f8e1)
[CentOS/Mac OS 免密ssh登录远程服务器](https://www.jianshu.com/p/9555795695b4)
[How to add your SSH public key to CentOS](http://kb.solarvps.com/centos/how-to-add-your-ssh-public-key-to-centos/)
