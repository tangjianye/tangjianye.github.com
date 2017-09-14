---
layout: post  
title:  Centos7搭建Gogs  
date:   2017-07-12 
tags: [Tools]  
categories: Tools  
author: JayaTang  
description: Centos7搭建Gogs代码仓库 
---
本文主要介绍搭建git仓库[Gogs](https://gogs.io/)。  

## 安装环境
- Git环境 `yum install git`
- ~~数据库环境(我采用的是默认sqlite3数据库)~~
- SSH 服务器(安装openssl,我部署的centos-7自带openssl)

## 创建新账号
- 虚拟机创建一个新账号：git     
Gogs需要部署在虚拟机git用户账号名下，否则会覆盖root用户下的ssh授权。 
```sh
[root@centos-7 ~]# adduser git
[root@localhost ~]# passwd git
```

- 授权  
新用户的权限只可以在本机home下有完整权限，其他目录要看别人授权。而经常需要root用户的权限，这时候sudo可以化身为root来操作。新创建的用户并不能使用sudo命令，需要给他添加授权。  
   
  1. sudo命令的授权管理是在sudoers文件里的：     
  ```sh
  [root@localhost ~]# sudoers    
  bash: sudoers: 未找到命令...    
  [root@localhost ~]# whereis sudoers   
  sudoers: /etc/sudoers /etc/sudoers.d /usr/libexec/sudoers.so /usr/share/man/man5/sudoers.5.gz   
  ```
  2. 找到这个文件位置之后再查看权限：   
  ```sh
  [root@localhost ~]# ls -l /etc/sudoers     
  -r--r----- 1 root root 4251 9月  25 15:08 /etc/sudoers    
  ```
  3. 只有只读的权限，如果想要修改的话，需要先添加w权限：    
  ```sh
  [root@localhost ~]# chmod -v u+w /etc/sudoers    
  mode of "/etc/sudoers" changed from 0440 (r--r-----) to 0640 (rw-r-----)  
  ```
  4. 然后就可以添加内容了，在下面的一行下追加新增的用户：    
  ```sh
  [root@localhost ~]# vim /etc/sudoers     
  # Allow root to run any commands anywher     
  root    ALL=(ALL)       ALL       
  git  ALL=(ALL)       ALL  #这个是新增的用户     
  ```
  5. wq保存退出，这时候要记得将写权限收回：     
  ```sh
  [root@localhost ~]# chmod -v u-w /etc/sudoers     
  mode of "/etc/sudoers" changed from 0640 (rw-r-----) to 0440 (r--r-----)   
  ```
  6. 这时候使用新用户登录，就可以使用sudo命令。
  ```sh
  [root@localhost ~]# su git     
  ```

## 安装Gogs  
本文介绍[二进制安装Gogs](https://gogs.io/docs/installation/install_from_binary.html)的方案。
- 下载系统对应的二进制文件    
```sh
[root@centos-7 ~]# wget https://dl.gogs.io/0.11.19/linux_amd64.zip
```
- 解压压缩包, 如果`unzip`命令不识别，先执行命令 `yum install -y unzip zip;`  
```sh
[root@centos-7 ~]# unzip linux_amd64.zip
```

## 启动Gogs

- 使用命令 cd 进入到刚刚解压的目录。
- 执行命令 `./gogs web`

## 后台运行与重启Gogs  
通过命令 `./gogs web`运行gogs是不可以关闭xshell界面的，如果需要后台运行需要如下配置      
```sh
[root@centos-7 ~]# su git
[root@centos-7 ~]# nohup ./gogs web &
```

## 配置Gogs
启动浏览器地址`ip:3000`就可以访问Gogs，第一次默认会进入`ip:3000/install`配置页面。具体的[配置](https://gogs.io/docs/advanced/configuration_cheat_sheet)官方网站介绍很详细。  
基本上都是默认配置就好了，此外需要注意的地方如下：
- 如果没有安装数据库记得数据库选择sqlite3版本。
- 服务器域名`DOMAIN`: 不要带`http`或者`https`。
- URL地址里面的`localhost`建议改成具体的虚拟机`ip地址`，否则下载代码的时候url路径可能都是`localhost`路径。
- 默认第一个注册用户就是管理员，不需要怀疑自己是管理员的身份。

运行成功页面显示如下：
![运行成功](/assets/img/centos-gogs/gogs.png)
