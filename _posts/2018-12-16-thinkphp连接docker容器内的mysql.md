---
layout:     post
title:      "thinkphp连接docker容器内的mysql"
subtitle:   "thinkphp连接docker容器内的mysql"
date:       2018-12-16 15:40:00
author:     "大可乐"
header-img: "img/post-docker-bg.png"
header-mask: 0.3
catalog:    true
tags:
  - thinkphp
  - php
  - mysql
  - docker
---

# thinkphp连接docker容器内的mysql

## 1.安装数据库

在  [phpdocker.io](phpdocker.io) 拉去数据库的配置.

执行命令

```
//ubuntu系统		安装需要一段时间静静等着就好
sudo docker-compose up
```

## 2.查看数据库映射到本地哪个端口

```
//查看docker容器
sudo docker ps -a
```

效果如下:

```linux
CONTAINER ID        IMAGE                                                  COMMAND                  CREATED             STATUS                     PORTS                    NAMES
bbc0d0127bad        nginx:alpine                                           "nginx -g 'daemon of…"   4 hours ago         Up About an hour           0.0.0.0:8088->80/tcp     thinkphp-webserver
30b449d312c5        thinkphp_php-fpm                                       "/bin/sh -c /usr/bin…"   4 hours ago         Up 4 hours                 9000/tcp                 thinkphp-php-fpm
3c3c81616be9        mysql:5.6                                              "docker-entrypoint.s…"   4 hours ago         Up 4 hours                 0.0.0.0:8090->3306/tcp   thinkphp-mysql

```

由此看到: mysql数据库映射到本机的8090

## 3.查看当前ip地址

```
ifconfig
```

效果:

```
wlp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.43.124  netmask 255.255.255.0  broadcast 192.168.43.255
        inet6 fe80::96c9:dbef:5980:ed12  prefixlen 64  scopeid 0x20<link>
        inet6 2409:8904:5225:19df:e15b:a3c1:ecb0:d2ec  prefixlen 64  scopeid 0x0<global>
        inet6 2409:8904:5225:19df:efac:3ffb:342b:176a  prefixlen 64  scopeid 0x0<global>
        ether 28:c2:dd:2a:44:91  txqueuelen 1000  (以太网)
        RX packets 406937  bytes 378304603 (378.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 321586  bytes 39618874 (39.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

由此看到本机的ip为192.168.43.124

## 4.thikphp中database.php中配置

```
<?php
return [
    // 数据库类型
    'type'            => 'mysql',
    // 服务器地址
    'hostname'        => '192.168.43.124',
    // 数据库名
    'database'        => 'thinkphp',
    // 用户名
    'username'        => 'root',
    // 密码
    'password'        => 'yuan',
    // 端口
    'hostport'        => '8090',
    // 连接dsn
    'dsn'             => '',
    // 数据库连接参数
    'params'          => [],
    // 数据库编码默认采用utf8
    'charset'         => 'utf8',
    // 数据库表前缀
    'prefix'          => 'think_',
    // 数据库调试模式
    'debug'           => true,
    // 数据库部署方式:0 集中式(单一服务器),1 分布式(主从服务器)
    'deploy'          => 0,
    // 数据库读写是否分离 主从式有效
    'rw_separate'     => false,
    // 读写分离后 主服务器数量
    'master_num'      => 1,
    // 指定从服务器序号
    'slave_no'        => '',
    // 自动读取主库数据
    'read_master'     => false,
    // 是否严格检查字段是否存在
    'fields_strict'   => true,
    // 数据集返回类型
    'resultset_type'  => 'array',
    // 自动写入时间戳字段
    'auto_timestamp'  => false,
    // 时间字段取出后的默认时间格式
    'datetime_format' => 'Y-m-d H:i:s',
    // 是否需要进行SQL性能分析
    'sql_explain'     => false,
    // Builder类
    'builder'         => '',
    // Query类
    'query'           => '\\think\\db\\Query',
    // 是否需要断线重连
    'break_reconnect' => false,
    // 断线标识字符串
    'break_match_str' => [],
];

```

