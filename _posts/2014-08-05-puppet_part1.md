---
layout: post
title: "Puppet学习(Part 1)"
description: ""
category: 运维技术
tags: [PUPPET, RUBY, 部署]
---
{% include JB/setup %}

# 理解误区
## root才能运行
master完全无需root，agent如果没有修改系统文件的需求也无需root。

由于很多时候agent需要用yum安装服务，所以用root会比较方便。

## 需要信任关系
agent和master完全是走http通信的，agent拿到master编译好的catlog后，直接在本地执行，并不依赖master。

## 不能主动运行
master可以通过kick命令强制agent运行。

agent也可以在服务外，单独运行—test来单次执行。

执行的内容可以通过tag控制。

## puppet是一个变更管理工具
puppet是一个运维基础环境管理工具，用puppet维护持续集成流程并非上策；

一些可能组合：

* puppet维护apache+tomcat环境，持续集成系统维护War包
* puppet维护运行环境和运维工具，持续集成系统维护product

从某种角度来说，puppet更像初始化平台，牛逼的是“**自动化和维持**”。

# 安装运行
## 安装
```
gem install puppet
```

## 配置文件路径：
root运行: `/etc/puppet`

非root运行： `~/.puppet`

## master运行
可以完全不做任何服务配置，只写manifests和modules。

## agent运行
`puppet.conf`中需要定义server的域名，或者启动时显示指定`--server`。

agent启动后，需要在master进行授权（`puppet cert …`）。

# Hello puppet
## master配置
```
# manifests/site.pp
node default {
  file { “/tmp/hello_puppet”:
    content => “hello puppet”;
  }
}
```
启动：`puppet master --verbose --no-daemonize`（前台启动，调试用）
## agent配置
```
# puppet.conf
[main]
  server = $master
```
启动：`puppet agent --no-daemonize —verbose`（前台启动，调试用）
## 授权
在master运行`puppet cert list`和`puppet cert sign $agent`
## 检查
slave上应该有`/tmp/hello_puppet`文件且内容为`hello puppet`

