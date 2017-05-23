---
layout: post  
title:  Android开发环境搭建
date:   2017-05-09 
tags: [Android, Environment]  
categories: Android  
author: JayaTang  
description: Android开发环境搭建。  
---
文档第一部分的基础环境搭建成功后，理论上已经可以进行android开发，其他工具和AS插件部分只是方便和优化开发工作而已。  

## 基础环境  

- [JDK下载](http://www.oracle.com/technetwork/java/javase/downloads/index.html)：建议下载JDK8版本，安装完成后需要设置Java环境变量。Java环境变量设置自行百度。  
![JDK](/assets/img/android-monkey/01.png)
- [Android Studio](https://developer.android.com/studio/index.html)：建议下载Android Studio2.3.1（自带SDK）版本。安装完成后设置Android环境变量（非必须）。    
![Android Studio](/assets/img/android-environment/01.png)  
- [Hosts](https://laod.cn/hosts/2017-google-hosts.html)：修复谷歌、Twitter和Facebook等访问的hosts。如果下载SDK或者Gradle失败，可以尝试替换系统hosts文件。
- [Gradle](https://services.gradle.org/distributions/): 如果AS下载gradle失败，手动下载gradle，配置环境变量（非必须）。 

按照步骤安装完成后，最基本的Android开发环境已经搭建成功，可以打开AS运行 Hello world!

## 其他工具  

- [Git](https://git-scm.com/downloads/)：代码管理工具。  
- [BeyondCompare](http://www.scootersoftware.com/download.php)：Diff工具，比较文件差异。可以置换AS、SourceTree、SVN等工具的默认比较工具使用。  
- [SourceTree](https://www.sourcetreeapp.com/)：一个windows和mac都可以使用的图形化Git工具。只是喜欢Git命令行的同事可以直接忽略。   

## AS插件

以下插件都是开发中非常实用的AS插件：  
- FindBugs-IDEA：代码静态检查工具插件。
- GsonFormat: 根据api接口生成相应的实体类插件。
- Android Selectors Generate：根据图片状态生成Drawable文件插件。
- Android Parcelable Code Generator：Parcelable自动序列化插件。
- Lifecycle Sorter：根据Activity或者fragment的生命周期对其生命周期方法位置进行先后排序。
- JsonOnlineViewer：网络请求、接口调试插件。
- [Folding-plugin](https://github.com/dmytrodanylyk/folding-plugin)：分类AS布局文件的神器。
- Genymotion：模拟器插件（低性能电脑建议搭配AS使用）。
