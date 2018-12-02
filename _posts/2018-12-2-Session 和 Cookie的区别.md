---
layout:     post
title:      "Session 和 Cookie的区别"
subtitle:   "Session 和 Cookie的区别"
date:       2018-12-2 11:40:00
author:     "大可乐"
header-img: "img/post-php-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
  - PHP
---

# Session 和 Cookie的区别

## 正常回答

cookie 和 session 都是会话技术,能够存储交互中的信息.

1. 存储位置不同.cookie是存储在客户端中,每次访问服务器时,都会带着当前域名下的cookie,而 session是存储在服务器端,占用服务器资源.
2. 存储限制不同,浏览器对cookie有个数限制,一般为单个域名最多存储50个,单个cookie最大值为4k,而session没有限制
3. 安全性方面.cookie的数据存储在客户端.容易被编辑伪造,所以相对来说不是特别的安全.
4. 请求速度方面.因为cookie每次都会传递给服务器.所以如果写入的cookie数据较多.就会导致报文体积增大,影响请求的速度.而session是通过sessionid来识别的.所以影响不大.
5. 生命周期方面.cookie的失效是即时的,但是session的失效是一定概率触发的.php默认的概率是1/1000,生命周期是24分钟.

## 深入回答

cookie 和 session都是会话技术,但是二者实现会话的原理不同.

### 原理不同

* cookie实现会话的原理,是每次都将cookie值存放在HTTP请求报文的请求头中,服务器可以接收到报文中的参数.比如用户id和用户名
* session 实现会话的原理,客户端请求服务器的时候,检测请求中是否存在session对应的cookie,该cookie的默认名字为PHPSESSION,如果不存在则生成一个随机的字符串.并生成相应的文件,进行session数据的读写.最后将该随机字符串写入到客户端的cookie中.如果存在则可以直接读取和写入数据

### 二者联系

**session的实现默认是依赖cookie的**,因为session id是默认存储在cookie中的,但是如果客户端禁用了cookie,session依然是可以用的,客户端只要将正确的 session id 传递给服务器即可.比如url参数,QQ的会员中心就是这么做的.

### 安全问题

session 和 cookie都存在一定的安全问题,因为两者都是基于HTTP请求的,HTTP请求一旦被截获,那么都好造成数据的丢失和一定程度的泄露,**建议服务器配置HTTPS证书,这样能有效的防止数据泄露,并且对cookie的数据进行加密处理,不要明文传递**

### session的跨服务器的问题(session共享)

PHP语言默认存储session的位置是tmp目录下,在服务器集群中就会出现session共享的问题,每个用户的session会存储在不同的服务器下,就会导致下次登录时,可能得不到自己的session,**此时可以将session存储到一台单独的服务器下,用mysql或者redis保存,** 每台服务器都到该服务器下去读取session即可.