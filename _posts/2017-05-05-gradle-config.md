---
layout: post  
title:  Gradle依赖的统一管理  
date:   2017-05-02 
tags: [Android]  
categories: Android  
author: JayaTang  
description: 利用Gradle进行Android版本和第三方依赖包的版本统一管理。  
---
对于一个团队开发不同的产品，Android版本和第三方依赖包的版本统一是一个问题，如果只是依靠代码review或者手动干预显然不是最科学的方式，如果可以做到利用Gradle进行Android版本和第三方依赖包的版本统一管理显然更科学。

## 本地Gradle文件配置
Android Studio通过本地Gradle文件配置灵活控制依赖包的版本。这个适用与在一个AS工程文件里面创建多个module的开发模式。

- 在AS工程文件的根目录创建'config/config.gradle'文件：  
![config.gradle](/assets/img/gradle-config-01.png)

- 配置好的config.gradle文件内容如下:  
{% gist a936c94f3181d4aea566787ea500a838 %}

- 在AS工程文件根目录build.gradle文件添加如下配置： 
![build.gradle](/assets/img/gradle-config-02.png)

- 子项目（module）的build.gradle文件的android版本和依赖库版本使用如下： 
![android版本](/assets/img/gradle-config-03.png)  
![依赖包版本](/assets/img/gradle-config-04.png)  

## 远程Gradle文件配置
Android Studio通过[远程Gradle文件配置](https://github.com/zmywly8866/ApplyRemoteGradleSample)文件灵活控制依赖包的版本。