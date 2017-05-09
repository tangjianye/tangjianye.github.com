---
layout: post  
title:  Android项目集成Sonar
date:   2017-05-09 
tags: [Tools]  
categories: Tools  
author: JayaTang  
description: Android项目集成SonarQube。  
---
代码检查工具能帮我们检查代码中隐藏的一些bug，[Sonar](https://www.sonarqube.org/)是其中一个不错的检查工具。  

## Sonar概述
以下介绍均来自网络：
>  Sonar 是一个用于代码质量管理的开放平台。通过插件机制，Sonar 可以集成不同的测试工具，代码分析工具，以及持续集成工具。与持续集成工具（例如 Hudson/Jenkins 等）不同，Sonar 并不是简单地把不同的代码检查工具结果（例如 FindBugs，PMD 等）直接显示在 Web 页面上，而是通过不同的插件对这些结果进行再加工处理，通过量化的方式度量代码质量的变化，从而可以方便地对不同规模和种类的工程进行代码质量管理。  
  在对其他工具的支持方面，Sonar 不仅提供了对 IDE 的支持，可以在 Eclipse 和 IntelliJ IDEA 这些工具里联机查看结果；同时 Sonar 还对大量的持续集成工具提供了接口支持，可以很方便地在持续集成中使用 Sonar。  
  此外，Sonar 的插件还可以对 Java 以外的其他编程语言提供支持，对国际化以及报告文档化也有良好的支持。  
  主要特点  
  - 代码覆盖：通过单元测试，将会显示哪行代码被选中
  - 改善编码规则
  - 搜寻编码规则：按照名字，插件，激活级别和类别进行查询
  - 项目搜寻：按照项目的名字进行查询
  - 对比数据：比较同一张表中的任何测量的趋势


## 环境
- [JDK8及以上环境](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)：需要设置环境变量。
- [SonarQube](https://www.sonarqube.org/downloads/)
- [SonarQube Scanner](http://repo1.maven.org/maven2/org/codehaus/sonar/runner/sonar-runner-dist/2.4/sonar-runner-dist-2.4.zip)：需要设置环境变量。
- ~~[数据库](https://dev.mysql.com/downloads/installer/)：非必须。~~

## Sonar的安装
只需要到 Sonar 网站下载最近的发行包即可，本文写作时最新的版本为6.3.1，下载 zip 包后，直接解压到任意目录，由于 Sonar 自带了 Jetty 6 的应用服务器环境，所以不需要额外的安装就可以使用，值得一提的是 Sonar 也支持部署在 Apache Tomcat 应用服务器中。  
- 在 windows 环境中，直接启动 Soanr 的 bin 目录下 windows-x86-64\StartSonar.bat 即可。
- 在 linux 环境中：NA  

然后在浏览器中访问：http://localhost:9000/ 即可  

1. 打开conf文件夹下的sonar.properties文件，设置登录账号密码：
```
sonar.sorceEncoding=UTF-8
sonar.login=admin
sonar.password=admin
```
2. 打开本地SonarQube页面[Quality Profiles -> Java, 2 profile(s) -> Android Lint], 配置Android Lint.
3. 运行cmd命令行，输入sonar-runner -version，看到sonar的版本信息就说明配置成功了。
4. 打开本地SonarQube页面[Administration -> Projects -> Management], 创建项目。项目的projetKey和projectName可以相同。

## 项目配置
1. 进入需要进行sonar分析的**Android项目**根目录，新建sonar-project.properties文件，然后配置下面信息，保存文件的编码需要是utf-8编码（[字段介绍](http://jingyan.baidu.com/article/e75057f2a2ae8eebc91a8935.html)
）：
```
sonar.projectKey=android-common
sonar.projectName=android-common
sonar.projectVersion=1.0
sonar.sources=app/src/main/java
sonar.binaries=app/build/intermediates/classes/  
sonar.language=java
sonar.sourceEncoding=UTF-8
sonar.profile=Android Lint
```
2. cmd进入项目所在的根目录，输入命令：sonar-runner，如果成功如下图：  
![sonar-runner成功](http://upload-images.jianshu.io/upload_images/1653520-5658fa600d3f9204.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
3. 浏览器访问: http://localhost:9000 点击项目，就可以查看具体问题了。  

本篇文章简单介绍了自己在本机搭建SonarQube环境时的步骤，不是完整的安装文档。搭建SonarQube环境时主要参考了两篇文章[《SonarQube的Android环境配置》](http://www.jianshu.com/p/826c80805bb2?nomobile=yes)和[《Android 代码检查工具SonarQube》](http://blog.csdn.net/z69183787/article/details/51502870)，不过遗憾的是如果只是按照两篇文章单独介绍的部分搭建环境都有问题，只有把两个文章里面介绍的内容进行整合才成功。