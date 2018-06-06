---
title: Linux常用命令
# date: 2018-06-05 23:56:24
tags: [linux]
---

# Linux常用命令
不断更新， 欢迎志同道合的一起学习- -

## 查看进程
```
ps -ef                  # 列出进程列表
ps -ef | grep nginx     # 在进程列表中搜索nginx
```bash
netstat -anp | grep :80
```

## 查看内核
```bash
rpm -qa | grep kernel
```

<!--more-->

## kill进程
```bash
kill -QUIT 主进程号       # 停止进程
kill -TEAM 主进程号       # 快速停止
kill -9 nginx            # 强制停止
```

## 防火墙
```bash
service iptables status  # 查看防火墙状态
service iptables start   # 开启防火墙
service iptables stop    # 关闭防火墙
```

