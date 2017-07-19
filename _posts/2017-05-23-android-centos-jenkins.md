---
layout: post  
title:  Centos搭建Android CI环境
date:   2017-05-23 
tags: [Android, CI]  
categories: Android  
author: JayaTang  
description: 本文档描述Centos 7利用Jenkins 搭建Android CI环境  
---
本文档描述Centos 7利用Jenkins 搭建Android CI环境。本机是在win10环境下利用Xshell 5 连接Centos服务器完成的全部操作。Centos服务器一定要是**root账号**登录。  
以下步骤基本上都是实际安装过程中的文字记录外加回忆和整理完成的。所以可能有些细节不一定完全准确。

## Java环境  

采用手动解压[JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)的压缩包，然后设置环境变量的方法。  

- 检查是否安装了openJava,如果安装了需要卸载  

- 打开/usr/目录   
  > [root@centos-7 ~]# cd /usr    

- 下载jdk,然后解压  
  > [root@centos-7 usr]# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz"    
  > [root@centos-7 usr]# tar -zxvf jdk-8u131-linux-x64.tar.gz       
  > [root@centos-7 usr]# mv jdk1.8.0_131 java    

- 设置环境变量  
  > [root@centos-7 usr]# vi /etc/profile   

  在profile文件尾添加如下内容：  
  ~~~sh
  # set java environment
  export JAVA_HOME=/usr/java
  export JRE_HOME=/usr/java/jre
  export CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
  ~~~

  刷新环境变量  
  > [root@centos-7 java]# source /etc/profile  

- 查看JDK安装是否成功    
  > [root@centos-7 java]# java -version   
  > java version "1.8.0_121"   
  > Java(TM) SE Runtime Environment (build 1.8.0_121)   
  > Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)   

## Android SDK环境  

采用手动解压[tools](https://developer.android.com/studio/index.html)的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建android-sdk-linux目录    
  > [root@centos-7 ~]# mkdir /usr/local/android-sdk-linux   
  > [root@centos-7 ~]# cd /usr/local/android-sdk-linux   

- 下载tools解压，然后下载对应的SDK版本    
  > [root@centos-7 android-sdk-linux]# wget https://dl.google.com/android/repository/tools_r25.2.3-linux.zip    
  > [root@centos-7 android-sdk-linux]# unzip tools_r25.2.3-linux.zip       
  > [root@centos-7 android-sdk-linux]# cd tools    
  > [root@centos-7 android-sdk-linux]# android update sdk --no-ui --all --filter platform,android-25,platform-tools,build-tools-25.0.2,extra-android-m2repository

- 设置环境变量    
  > [root@centos-7 android-sdk-linux]# vi /etc/profile   

  在profile文件尾添加如下内容：  
  ~~~sh
  # set android sdk
  export ANDROID_HOME=/usr/local/android-sdk-linux
  export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
  ~~~

  刷新环境变量
  > [root@centos-7 android-sdk-linux]# source /etc/profile    

- 查看SDK安装是否成功  
  > [root@centos-7 android-sdk-linux]# adb   

## Gradle环境  

采用手动解压[Gradle](https://services.gradle.org/distributions)的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建gradle目录  
  > [root@centos-7 ~]# mkdir /usr/local/gradle   
  > [root@centos-7 ~]# cd /usr/local/gradle   

- 下载gradle3.3解压  
  > [root@centos-7 gradle]# wget https://services.gradle.org/distributions/gradle-3.3-all.zip    
  > [root@centos-7 gradle]# unzip gradle-3.3-all.zip    

- 设置环境变量  
  > [root@centos-7 gradle]# vi /etc/profile   

  在profile文件尾添加如下内容：  
  ~~~sh
  # set gradle
  export GRADLE_HOME=/usr/local/gradle/gradle-3.3
  export PATH=$PATH:$GRADLE_HOME/bin
  ~~~

  刷新环境变量  
  > [root@centos-7 gradle]# source /etc/profile    

- 查看Gradle安装是否成功    
  > [root@centos-7 gradle]# gradle -v     
  > [root@centos-7 gradle]# Gradle 3.3  



现在centos上就已经搭建好了android app 编译环境，可以利用'git clone xxx项目'。然后进入xxx项目执行'gradle assembleRelease'查看编译是否成功。


## Tomcat7环境  

采用手动解压[Tomcat](http://tomcat.apache.org/)的压缩包，然后设置环境变量的方法。  

- 在/usr/local目录下创建tomcat目录    
  > [root@centos-7 ~]# mkdir /usr/local   
  > [root@centos-7 ~]# cd /usr/local   

- 下载tools解压，然后下载对应的SDK版本   
  > [root@centos-7 local]# wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-7/v7.0.78/bin/apache-tomcat-7.0.79.tar.gz    
  > [root@centos-7 local]# tar -zxvf  apache-tomcat-7.0.79.tar.gz      
  > [root@centos-7 local]# mv apache-tomcat-7.0.79 tomcat      


- 设置环境变量    
  > [root@centos-7 tomcat]# vi /etc/profile   

  在profile文件尾添加如下内容：  
  ~~~sh
  # set tomcat
  export TOMCAT_HOME=/usr/local/tomcat
  ~~~

  刷新环境变量
  > [root@centos-7 tomcat]# source /etc/profile    

- 启动Tomcat  
  > [root@centos-7 tomcat]# cd /usr/local/tomcat/apache-tomcat-7.0.78/bin   
  > /usr/local/tomcat exists, start tomcat server!    
  > Using CATALINA_BASE:   /usr/local/tomcat     
  > Using CATALINA_HOME:   /usr/local/tomcat     
  > Using CATALINA_TMPDIR: /usr/local/tomcat/temp     
  > Using JRE_HOME:        /usr/java/jre     
  > Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar     
  > Tomcat started.      
  > Open firewall 8080 port.     


- 设置UTF-8编码
  修改tomcat下的conf/server.xml文件
  ```xml
  <Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" URIEncoding="UTF-8" />
  ```
- 登录Tomcat  

  打开浏览器查看http://localhost:8080 是否出现Tomcat 页面即可验证是否配置成功。如果失败一般是防火墙的问题。 
  > 1）关闭firewall：  
  > [root@centos-7 bin]# systemctl stop firewalld.service #停止firewall  
  > [root@centos-7 bin]# systemctl disable firewalld.service #禁止firewall开机启动  
  > [root@centos-7 bin]# firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）    
  > 2）防火墙开放默认的8080端口    
  > [root@centos-7 bin]# firewall-cmd --permanent --zone=public --add-port=8080/tcp   
  > [root@centos-7 bin]# firewall-cmd --reload   


## Jenkins环境

采用手动下载[Jenkins](https://jenkins.io/download/)的压缩包。

- 打开tocat/webapps目录    
  > [root@centos-7 ~]# mkdir /usr/local/tomcat/apache-tomcat-7.0.78/webapps   

- 下载jenkins.war    
  > [root@centos-7 gradle]# wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war     

- 设置环境变量    
  > [root@centos-7 gradle]# vi /etc/profile   

  在profile文件尾添加如下内容：    
  ~~~sh
  # set jenkins
  export JENKINS_HOME=/usr/local/tomcat/apache-tomcat-7.0.78/webapps/jenkins
  ~~~

  刷新环境变量
  > [root@centos-7 gradle]# source /etc/profile    

- 运行Jenkins    
  启动tomcat就可以直接运行jenkins  
  > [root@centos-7 tomcat]# cd /usr/local/tomcat/apache-tomcat-7.0.78/bin     
  > [root@centos-7 bin]# ./startup.sh    

  关闭tomcat命令   
  > [root@centos-7 bin]# ./shutdown.sh    


- 登录Jenkins   
  打开浏览器查看http://localhost:8080/jenkins 就可以登录。  


## Curl环境

采用手动解压[curl](https://curl.haxx.se/download.html)的压缩包，然后设置环境变量的方法。安装curl是方便app编译完成后发布到蒲公英/fir.im等第三方APP托管平台。  

- 在/usr/local目录下创建curl目录   
  > [root@centos-7 ~]# mkdir /usr/local/curl    
  > [root@centos-7 ~]# cd /usr/local/curl    

- 下载curl解压  
  > [root@centos-7 curl]# wget http://curl.haxx.se/download/curl-7.20.0.tar.gz     
  > [root@centos-7 curl]# tar -zxvf curl-7.20.0.tar.gz   
  > [root@centos-7 curl]# cd curl-7.17.1     
  > [root@centos-7 curl]# ./configure --prefix=/usr/local/curl   
  > [root@centos-7 curl]# make    
  > [root@centos-7 curl]# make install  


- 设置环境变量  
  > [root@centos-7 gradle]# vi /etc/profile   

  在profile文件尾添加如下内容：  
  ~~~sh
  # set curl
  export PATH=$PATH:/usr/local/curl/bin
  ~~~

  刷新环境变量  
  > [root@centos-7 gradle]# source /etc/profile    

- 查看curl安装是否成功    
> [root@centos-7 gradle]# curl --help      
 

- 安装curl失败可能是还需要安装gcc: `yum install gcc` 。

## Jenkins配置

- 系统配置  

![系统配置](/assets/img/android-centos-jenkins/sys-config-01.png)      

![系统配置](/assets/img/android-centos-jenkins/sys-config-02.png)    

![系统配置](/assets/img/android-centos-jenkins/sys-config-03.png)    

![系统配置](/assets/img/android-centos-jenkins/sys-config-04.png)    

![系统配置](/assets/img/android-centos-jenkins/sys-config-05.png)    

- Job配置

![Job配置](/assets/img/android-centos-jenkins/job-config-01.png)  

![Job配置](/assets/img/android-centos-jenkins/job-config-02.png)    

![Job配置](/assets/img/android-centos-jenkins/job-config-03.png)    

![Job配置](/assets/img/android-centos-jenkins/job-config-04.png)     

![Job配置](/assets/img/android-centos-jenkins/job-config-05.png)    

![Job配置](/assets/img/android-centos-jenkins/job-config-06.png)  


## 附录

- 环境变量 `/etc/profile`  
  实际搭建环境过程中的环境变量值（和文档上面的描述有一点点偏差），文档上面的描述是我认为更加合理的模式    
  ~~~sh
  #set java environment
  export JAVA_HOME=/usr/java
  export JRE_HOME=/usr/java/jre
  export CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

  # set android sdk
  export ANDROID_HOME=/usr/local/android-sdk-linux
  export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

  ## set gradle
  export GRADLE_HOME=/usr/local/gradle/gradle-3.3
  export PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin:${GRADLE_HOME}/bin:${JAVA_HOME}:${PATH}

  # set jenkins
  export JENKINS_HOME=/usr/local/apache-tomcat-7.0.78/webapps/jenkins

  # set tomcat
  export TOMCAT_HOME=/usr/local/apache-tomcat-7.0.78

  # set curl
  export PATH=$PATH:/usr/local/curl/bin  
  
  # set sonar
  export SONAR_HOME=/usr/local/sonarqube-5.6.6
  export SONAR_RUNNER_HOME=/usr/local/sonar-runner-2.4
  export PATH=$SONAR_RUNNER_HOME/bin:$PATH 
  ~~~
  
- 蒲公英上传 `jenkins-build.sh`  
  ~~~sh
  #!/bin/sh

  echo "===========发布到蒲公英==========="
  #蒲公英上的User Key
  uKey="a04fb657256887f1f7a28f09bad93856"
  #蒲公英上的API Key
  apiKey="f8b02896cfce9e52814172a6d8f267e7"
  #${WORKSPACE}Jenkins下载的项目地址 
  curl -F "file=@${WORKSPACE}/platform/build/outputs/apk/aorise-petition-release.apk" \
    -F "uKey=${uKey}" -F "_api_key=${apiKey}" http://www.pgyer.com/apiv1/app/upload
  echo "\n\n"
  ~~~
  
  
## Q&A

- 如何在centos系统下载各种软件？  
  > - 利用wget 命令  
  > ![wget命令](/assets/img/android-centos-jenkins/download-jdk.png)  
  > - 利用xshell 的 rz sz 命令(不建议)  

- Jenkins邮箱配置后邮件发送失败？  
  > - 邮箱的配置一直用同一个账号  
  > - 邮箱开通smtp协议   
  > - 用LTS版本的Jenkins  