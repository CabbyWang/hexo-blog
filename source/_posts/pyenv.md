---
title: pyenv的安装及使用
# date: 2018-06-05 23:56:24
tags: [pyenv, pyenv-virtualenv]
---


# pyenv和pyenv virtualenv
[这里是pyenv的gitbhu地址](https://github.com/pyenv/pyenv)
[这里是pyenv-virtualenv的github地址](https://github.com/pyenv/pyenv-virtualenv)
## 安装
安装比较简单，以下是mac的安装方式
- 安装pyenv
```bash
brew install pyenv
```
- 安装pyenv virtualenv
```bash
brew install pyenv-virtualenv
```
<!--more-->

```bash
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## 使用
- pyenv常用操作
```bash
pyenv version       # pyenv版本
pyenv versions      # 查看已经安装的python版本
pyenv install --list  # 查看可安装列表
# 这里可以加个查询，如下
pyenv install --list | grep "3.6"
pyenv install 3.6.5    # 安装python3.6.5
# 安装好了当然是开始使用
pyenv global 3.6.5
```

- pyenv-virtualenv常用操作
```bash
pyenv virtualenv 3.6.5 python3  # 这里创建了一个名为python3的虚拟环境(3.6.5是指python版本为3.6.5)
pyenv activate python3          # 激活虚拟环境python3
pyenv deactivate                # 关闭虚拟环境
pyenv uninstall python3         # 卸载python3的虚拟环境`
```
