---
layout: post  
title:  Jenkins节点配置  
date:   2017-07-21 
tags: [Tools]  
categories: Tools  
author: JayaTang  
description: Jenkins主从节点配置 
---
文档主要介绍Jenkins主从节点配置，文档的前提条件是有两台centos7虚拟机，一台配置master主机，一台配置slave主机。具体的Jenkins配置请参考另外一篇文章[<Centos搭建Android CI环境>](https://tangjianye.github.io/android/2017/05/23/android-centos-jenkins)。现在以主机已经搭建最小化的Jenkins环境，从机已经搭建android编译环境为例，介绍Jenkins节点配置。 

## 环境介绍
- 主机环境介绍：主机Jenkins运行在tomcat中。Jenkins本身安装的环境仅包括java环境和gradle环境。
  ```bash
  # set java environment
  export JAVA_HOME=/usr/java
  export JRE_HOME=/usr/java/jre
  export CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

  # set gradle
  export GRADLE_HOME=/usr/local/gradle/gradle-3.3
  export PATH=$PATH:$GRADLE_HOME/bin

  # set tomcat
  export TOMCAT_HOME=/usr/local/tomcat/apache-tomcat-7.0.79

  # set jenkins
  export JENKINS_HOME=/usr/local/tomcat/apache-tomcat-7.0.79/webapps/jenkins
  ```
- 从机环境介绍：从机Jenkins运行在tomcat中。Jenkins本身安装的环境不仅包括java环境和gradle环境，还包括android编译环境。实际上从机本身启动的Jenkins页面是可以完整独立的编译android应用源码。
  ```bash
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
  ```

## 主从机通信
利用ssh keygen产生公钥和私钥对，将公钥拷贝到远程主机的authorized_keys文件中（或者通过ssh-copy-id命令），这样就可以通过ssh user@ip登录远程主机了。
> 主机产生公钥和私钥对，从机作为远程主机。  

- 创建公钥私钥对：主机输入`ssh-keygen -t rsa`一路回车即可。
- 拷贝公钥至远程主机：手动复制主机id_rsa.pub中的内容，写入从机（远程主机）authorized_keys中。~~或者通过 `ssh-copy-id root@远程主机IP` 实现（我没有成功）。~~
- 测试登录：主机通过命令行 `ssh  root@远程主机IP` 测试是否可以成功登陆远程主机。

## 从机配置
从机理论上如果之前安装jenkins配置好了，其实已经不需要配置了。只是 `Global Tool Configuration` 如果作为从机，就一定要配置才可以编译通过。 
![环境变量配置](/assets/img/jenkins-master-slave/slave.png)  


## 主机配置

### 环境变量配置
和从机配置一样。   

### 节点配置    
在系统管理 / 节点管理 创建新节点。      
![创建节点](/assets/img/jenkins-master-slave/master-node.png)   
点击新建节点，进行节点配置。       
![配置节点](/assets/img/jenkins-master-slave/master.png)    

### 使用节点
项目中使用slave节点进行编译。   
![使用节点](/assets/img/jenkins-master-slave/configs.png)   

## 注意事项

- 主从机不能够识别？
> 主从机通信一定要手动通过 `ssh  root@远程主机IP` 测试通过。如果还是不行，就把主从机相互设置都可以远程访问。

- 从机可以独立编译通过，通过主机编译就不可以？
 > - 主机项目是否设置了slave节点编译？
 > - 主从机是否安装了相同的软件版本，`Global Tool Configuration` 是否设置了相同的环境变量和路径？

- 从机sh脚本可以独立编译通过，通过主机编译就不可以？
> 这个问题我有一个不太科学的解决方案，就是sh脚本里面的命令用slave机里面的绝对路径来实现。
>    >   例如：gradle sonarqube;  // 不行     
>    >        /usr/local/gradle/gradle-3.3/bin/gradle sonarqube;   // 可以