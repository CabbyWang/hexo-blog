---
title: centerOS部署hexo
# date: 2018-06-05 23:56:24
tags: [hexo, centerOS]
---


# hexo部署到centerOS
之前使用github搭建过hexo博客，最近租了个vps, 刚好想着折腾一下将hexo搭建在自己的云服务器上， 便有了这篇文章，感觉写的不太好， 若是哪里有问题， 欢迎指出； 哪里有不懂的， 可以一起交流， 相互学习……^_^

## 服务器操作
安装nginx
```bash
yum install nginx -y
```
启动nginx
```bash
service nginx start
```
防火墙添加80端口（nginx默认80端口）
```bash
filewall-cmd --zone=public --add-port=80/tcp --permanent
```
<!--more-->
reload防火墙
```bash
filewall-cmd --reload
```
查看防火墙端口
```bash
filewall-cmd --list-ports
```
使用浏览器访问服务器ip

或在网页输入访问域名，出现下图成功
![](http://p8c9hbb6s.bkt.clouddn.com/nginx%E9%BB%98%E8%AE%A4.png)
修改nginx.conf
```bash
server {
     listen       80 default_server;
     listen       [::]:80 default_server;  # 默认监听端口
     server_name  wangsiyong.cc www.wangsiyong.cc; # 域名(没有域名使用ip地址)
     root         /data/www/hexo; # 这里就是外网默认的访问路径，也就是hexo的根目录。
 }
```

测试虚拟主机运行情况(在刚才配置的外网默认访问路径下创建index.html)
```bash
vim /data/www/hexo/index.html
# 输入
<html>
	<h1>Hello World!</h1>
</html>
```
网页输入域名， 出现下图成功
![](http://p8c9hbb6s.bkt.clouddn.com/nginx%E6%9C%8D%E5%8A%A1%E6%B5%8B%E8%AF%95.png)

在服务器创建一个仓库(bare)
```bash
mkdir /data/hexo_blog/hexo.git -p
git init --bare         # bare repository
cd hooks
```
创建git钩子(hooks)
```bash
vim post_receive
```
在`post_receive`中添加
```bash
#!/bin/bash
git --work-tree=/data/www/hexo --git-dir=/data/hexo_blog/hexo.git checkout -f    # 第一个路径是nginx.conf中配置的外网默认访问路径（可以往上看）， 第二个路径是git仓库所在的路径（也就是前一步创建的仓库）
```
保存退出，然后修改`post_receive`文件的全选
```bash
chmod +x /data/hexo_blog/hexo.git/hooks/post_receive
```
到这里服务器配置就完成了
***

## 本机操作
安装git
```bash
yum install git -y
```

***
安装nodejs
```bash
yum install nodejs -y
```
***
安装hexo
```bash
npm install hexo-cli -g    # 全局安装hexo
```

```bash
mkdir -p ~/hexo_blog       # 用户目录下创建hexo_blog目录
cd ~/hexo_blog
hexo init
```
***

修改`hexo_blog`目录下的`_config.yml`文件(这里只展示url和deploy部分)
```bash
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
#url: http://yoursite.com/
url: wangsiyong.cc            # 服务器外网默认访问路径（前面已经配置过）
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git     # 以git的方式
  repo: root@202.xx.xx.xx:/data/hexo_blog/hexo.git   # 服务器地址:服务器上的git仓库
  branch: master  # master分支
```
配置完， 然后就是部署了
```bash
cd ~/hexo_blog
npm install hexo-deployer-git --save
hexo g
hexo d
```
然后就可以访问自己的域名欣赏自己的博客啦， hexo的其他配置这里就不介绍了- -
