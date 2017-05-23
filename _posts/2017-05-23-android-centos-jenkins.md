---
layout: post  
title:  Centos搭建Android CI环境
date:   2017-05-09 
tags: [Android, CI]  
categories: Android  
author: JayaTang  
description: 本文档描述Centos 7利用Jenkins 搭建Android CI环境  
---
本文档描述Centos 7利用Jenkins 搭建Android CI环境。本机是在win10环境下利用Xshell 5 连接Centos服务器完成的全部操作。Centos服务器一定要是**root账号**登录。  
以下步骤基本上都是实际安装过程中的文字记录外加回忆和整理完成的。所以可能有些细节不一定完全准确。

## Java环境  

采用手动解压JDK的压缩包，然后设置环境变量的方法。  

- 检查是否安装了openJava,如果安装了需要卸载  

- 在/usr/目录下创建java目录  
> [root@localhost ~]# mkdir /usr/java 
> [root@localhost ~]# cd /usr/java  

- 下载jdk,然后解压  
> [root@localhost java]# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz"
> [root@localhost java]# tar -zxvf jdk-7u79-linux-x64.tar.gz 

- 设置环境变量  
  > [root@localhost java]# vi /etc/profile 

  在profile文件尾添加如下内容：  
  ~~~sh
  #set java environment
  export JAVA_HOME=/usr/java/jdk1.8.0_121
  export JRE_HOME=/usr/java/jdk1.8.0_121/jre
  export CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
  ~~~

  刷新环境变量
  > [root@localhost java]# vi /etc/profile  

- 查看JDK安装是否成功  
> [root@localhost java]# java -version 
> java version "1.8.0_121" 
> Java(TM) SE Runtime Environment (build 1.8.0_121) 
> Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode) 

## Android SDK环境  

采用手动解压tools的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建android-sdk-linux目录  
> [root@localhost ~]# mkdir /usr/local/android-sdk-linux 
> [root@localhost ~]# cd /usr/local/android-sdk-linux 

- 下载tools解压，然后下载对应的SDK版本  
> [root@localhost android-sdk-linux]# wget https://dl.google.com/android/repository/tools_r25.2.3-linux.zip  
> [root@localhost android-sdk-linux]# unzip tools_r25.2.3-linux.zip     
> [root@localhost android-sdk-linux]# cd xxxxxx/bin  
> [root@localhost android-sdk-linux]# android update sdk --no-ui --all --filter platform,android-25,platform-tools,build-tools-25.0.2,extra-android-m2repository

- 设置环境变量  
  > [root@localhost android-sdk-linux]# vi /etc/profile 

  在profile文件尾添加如下内容：  
  ~~~sh
  # set android sdk
  export ANDROID_HOME=/usr/local/android-sdk-linux
  export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
  ~~~

  刷新环境变量
  > [root@localhost android-sdk-linux]# vi /etc/profile  

- 查看SDK安装是否成功  
> [root@localhost android-sdk-linux]# adb 
> [root@localhost android-sdk-linux]# adb ...

## Gradle环境  

采用手动解压[Gradle](https://services.gradle.org/distributions)的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建gradle目录  
> [root@localhost ~]# mkdir /usr/local/gradle 
> [root@localhost ~]# cd /usr/local/gradle 

- 下载gradle3.3解压  
> [root@localhost gradle]# wget https://services.gradle.org/distributions/gradle-3.3-all.zip  
> [root@localhost gradle]# unzip gradle-3.3-all.zip  

- 设置环境变量  
  > [root@localhost gradle]# vi /etc/profile 

  在profile文件尾添加如下内容：  
  ~~~sh
  ## set gradle
  export GRADLE_HOME=/usr/local/gradle/gradle-3.3
  export PATH=$PATH:$GRADLE_HOME/bin
  ~~~

  刷新环境变量
  > [root@localhost gradle]# vi /etc/profile  

- 查看Gradle安装是否成功  
> [root@localhost gradle]# gradle -v   
> [root@localhost gradle]# Gradle 3.3



现在centos上就已经搭建好了android app 编译环境，可以利用'git clone xxx项目'。然后进入xxx项目执行'gradle assembleRelease'查看编译是否成功。

---

## Tomcat7环境  

采用手动解压[Tomcat](http://tomcat.apache.org/download-70.cgi)的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建tomcat目录  
> [root@localhost ~]# mkdir /usr/local/tomcat 
> [root@localhost ~]# cd /usr/local/tomcat 

- 下载tools解压，然后下载对应的SDK版本  
> [root@localhost tomcat]# wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-7/v7.0.78/bin/apache-tomcat-7.0.78.tar.gz
> [root@localhost tomcat]# tar -zxvf  apache-tomcat-9.0.0.M17.tar.gz  

- 设置环境变量  
  > [root@localhost tomcat]# vi /etc/profile 

  在profile文件尾添加如下内容：  
  ~~~sh
  ## set tomcat
  export TOMCAT_HOME=/usr/local/tomcat/apache-tomcat-7.0.78
  ~~~

  刷新环境变量
  > [root@localhost tomcat]# vi /etc/profile  

- 启动Tomcat  
> [root@localhost tomcat]# cd /usr/local/tomcat/apache-tomcat-7.0.78/bin   
> [root@localhost bin]# ./startup.sh  
> Using CATALINA_BASE:   /usr/local/tomcat/apache-tomcat-7.0.78  
> Using CATALINA_HOME:   /usr/local/tomcat/apache-tomcat-7.0.78  
> Using CATALINA_TMPDIR: /usr/local/tomcat/apache-tomcat-7.0.78/temp  
> Using JRE_HOME:        /usr/java/jdk1.8.0_121  
> Using CLASSPATH:       /usr/local/tomcat/apache-tomcat-7.0.78/bin/bootstrap.jar:/opt/apache-tomcat-8.0.23/bin/tomcat-juli.jar  
> Tomcat started.  

- 登录Tomcat  

  打开浏览器查看http://localhost:8080 是否出现Tomcat 页面即可验证是否配置成功。如果失败一般是防火墙的问题。
  > 防火墙开放默认的8080端口
  > [root@localhost bin]# firewall-cmd --permanent --zone=public --add-port=8080/tcp
  > [root@localhost bin]# firewall-cmd --reload


## Jenkins环境

采用手动下载[Jenkins](https://jenkins.io/download/)的压缩包。

- 打开tocat/webapps目录  
> [root@localhost ~]# mkdir /usr/local/tomcat/apache-tomcat-7.0.78/webapps 

- 下载jenkins.war    
> [root@localhost gradle]# wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo 

- 设置环境变量  
  > [root@localhost gradle]# vi /etc/profile 

  在profile文件尾添加如下内容：  
  ~~~sh
  # set jenkins
  export JENKINS_HOME=/usr/local/tomcat/apache-tomcat-7.0.78/webapps/jenkins
  ~~~

  刷新环境变量
  > [root@localhost gradle]# vi /etc/profile  

- 运行Jenkins  
  启动tomcat就可以直接运行jenkins
  > [root@localhost tomcat]# cd /usr/local/tomcat/apache-tomcat-7.0.78/bin   
  > [root@localhost bin]# ./startup.sh  

  关闭tomcat命令 
  > [root@localhost bin]# ./shutdown.sh  


- 登录Jenkins  
打开浏览器查看http://localhost:8080/jenkins 就可以登录。
