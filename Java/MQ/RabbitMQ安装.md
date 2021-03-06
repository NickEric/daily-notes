---
title: Linux下安装RabbitMQ
tags:
  - Linux
categories:
  - Linux
date: 2019-01-21 22:00:00
---

本章主要讲了如何通过解压方式在Linux下安装RabbitMQ和Erlang，超级详细的安装过程，和安装过程中遇到的各种坑。

<!-- more-->

> 点击阅读更多Linux入门系列文章[欢迎访问我的个人博客-->幻境云图](https://www.lixueduan.com/categories/Linux/)

软件统一放在`/usr/software`下 解压后放在单独的文件夹下`/usr/locac/opt/rabbitmq`,`/usr/local/opt/erlang`

## 0. 版本问题

Erlang和RabbitMQ版本必须对应才行，不然可能会出错。

具体信息在这里`http://www.rabbitmq.com/which-erlang.html`

这里选择的版本是 `Erlang:21.2`,`RabbitMQ3.7.10`,`Linux:CentOS 7`

RabbitMQ Erlang Version Requirements

Introduction

This guide covers Erlang/OTP version requirements, Erlang version support policy, a RabbitMQ/Erlang compatibility matrix, version-specific notes and ways of provisioning recent Erlang/OTP releases.

Unsupported Versions

Erlang/OTP versions **older than 20.3 are not supported** by RabbitMQ versions released in 2019.

RabbitMQ **versions prior to 3.7.7 do not support Erlang/OTP 21** or newer.

Supported Erlang Version Policy

Starting in January 2019, RabbitMQ supports two most recent Erlang release series. Currently the series are 20.3.x and 21.x. When Erlang 22.0 ships, after a 3 month transition period, the supported versions will be 21.2.x and 22.x.

The table below provides an Erlang compatibility matrix of currently supported RabbitMQ release series. For RabbitMQ releases that have reached end of life, see Unsupported Series Compatibility Matrix.

RabbitMQ and Erlang/OTP Compatibility Matrix

| RabbitMQ version                          | Minimum required Erlang/OTP | Maximum supported Erlang/OTP | Notes                                                        |
| :---------------------------------------- | --------------------------- | ---------------------------- | ------------------------------------------------------------ |
| **3.7.10**,**3.7.9**,**3.7.8**,**3.7.7**  | **20.3.x**                  | **21.x**                     | Erlang/OTP 19.3.x support is discontinued as of Jan 1st, 2019For the best TLS support, the latest version of Erlang/OTP 21.x is recommendedOn Windows, Erlang/OTP 20.2 changed default cookie file location |
| 3.7.6,3.7.5,3.7.4,3.7.3,3.7.2,3.7.1,3.7.0 | 19.3                        | 20.3.x                       | For the best TLS support, the latest version of Erlang/OTP 20.3.x is recommendedErlang versions prior to 19.3.6.4 have known bugs (e.g. ERL-430  that can prevent RabbitMQ nodes from accepting connections (including from CLI tools) and stoppingVersions prior to 19.3.6.4 are vulnerable to the ROBOT attack(CVE-2017-1000385)On Windows, Erlang/OTP 20.2 changed default cookie file location |

As a rule of thumb, most recent patch versions of each supported Erlang/OTP series is recommended.

## 1. Erlang安装

### 1.1 下载

安装RabbitMQ之前需要先安装Erlang.

下载地址：`http://www.erlang.org/downloads`

文件`otp_src_21.2.tar.gz`

### 1.2 解压

将压缩包上传到虚拟机中，我是放在/usr/software目录下的

`# tar xvf otp_src_21.2.tar.gz`  解压文件

创建erlang安装目录： /usr/local/opt/erlang   可以不复制

`# cp -r otp_src_21.2 /usr/local/opt/erlang/`将文件复制进去

### 1.3 编译

进入到/usr/local/opt/erlang目录下

配置安装路径编译代码：`# ./configure --prefix=/usr/local/opt/erlang`

`# make && make install` 执行编译并安装

### 1.4 环境变量配置

配置Erlang环境变量,`# vi /etc/profile` 添加以下内容

```xml
export PATH=$PATH:/usr/local/opt/erlang/bin
```

 `# source /etc/profile `使得文件生效

### 1.5 验证

验证erlang是否安装成功：`# erl` 进入如下界面就说明 配置好了

```xml
[root@localhost bin]# erl
Erlang/OTP 21 [erts-10.2] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1]

Eshell V10.2  (abort with ^G)
1> 
`
```

## 2. RabbitMQ安装

### 2.0 依赖下载

`# yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel`

`# yum install python -y`

`# yum install xmlto -y`

`# yum install python-simplejson -y`



### 2.1 下载

地址：`http://www.rabbitmq.com/releases/rabbitmq-server`

这里下载的时3.7.10-->`http://www.rabbitmq.com/install-generic-unix.html`

文件：`rabbitmq-server-generic-unix-3.7.10.tar.xz`

### 2.2 解压

文件是xz格式的，解压后得到tar格式文件。

`# xz -d rabbitmq-server-generic-unix-3.7.10.tar.xz`

`# tar -xvf rabbitmq-server-generic-unix-3.7.10.tar`

复制到/usr/local/opt/rabbitmq目录下`# cp -r rabbitmq_server-3.7.10/ /usr/local/opt/rabbitmq`

### 2.3 环境变量配置

配置rabbitmq环境变量,`# vi /etc/profile` 添加以下内容

`export PATH=$PATH:/usr/local/opt/rabbitmq/sbin`

环境变量生效：`source /etc/profile`



### 2.4 使用

进入/usr/local/opt/rabbitmq/sbin目录

启动服务：`# ./rabbitmq-server -detached`

查看服务状态：`# ./rabbitmqctl status`

关闭服务：`# ./rabbitmqctl stop `

### 2.5 配置网页插件

首先创建目录，否则可能报错：mkdir /etc/rabbitmq 

启用插件：./rabbitmq-plugins enable rabbitmq_management

启动mq：./rabbitmq-server -detached

端口： 15672 网页管理，  5672 AMQP端口

然后访问`http://192.168.5.154:15672/`

rabbitmq默认会创建guest账号，只能用于localhost登录页面管理员

进入sbin

启动mq:

cd opt/rabbitmq/sbin

./rabbitmq-server –detached

 

查看服务状态：rabbitmqctl status

关闭服务：rabbitmqctl stop

查看mq用户：rabbitmqctl list_users  

查看用户权限：rabbitmqctl list_user_permissions guest

新增用户： `# rabbitmqctl add_user root root`  用户名root,密码root

赋予管理员权限：

`rabbitmqctl set_user_tags root administrator `

`rabbitmqctl set_permissions -p "/" root ".*" ".*" ".*" `



## 3. 问题

1.启动报错

```java
[root@localhost sbin]# ./rabbitmq-server start

BOOT FAILED
===========
=INFO REPORT==== 21-Jan-2019::20:49:29.302765 ===
Error description:
   noproc
   
Log files (may contain more information):
   /usr/local/opt/rabbitmq/var/log/rabbitmq/rabbit@localhost.log
   /usr/local/opt/rabbitmq/var/log/rabbitmq/rabbit@localhost-sasl.log

Stack trace:
   [{gen,do_for_proc,2,[{file,"gen.erl"},{line,228}]},
    {gen_event,rpc,2,[{file,"gen_event.erl"},{line,239}]},
    {rabbit,ensure_working_log_handlers,0,
            [{file,"src/rabbit.erl"},{line,856}]},
    {rabbit,'-boot/0-fun-0-',0,[{file,"src/rabbit.erl"},{line,288}]},
    {rabbit,start_it,1,[{file,"src/rabbit.erl"},{line,424}]},
    {init,start_em,1,[]},
    {init,do_boot,3,[]}]

{"init terminating in do_boot",noproc}
init terminating in do_boot (noproc)

Crash dump is being written to: erl_crash.dump...done
```

这个问题网上查了一下，有的说是权限问题，也有说是erlang和rabbitmq版本对应不上，暂时没解决。

已解决，确实是版本问题，erlang版本和rabbitmq的版本对应不上。最前面单独写了这个关于版本的问题。