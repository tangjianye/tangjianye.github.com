---
layout: post  
title:  Monkey测试  
date:   2017-03-07  
tags: [Android, Test]  
categories: Android  
author: JayaTang  
description: Monkey测试是Android平台自动化测试的一种手段。  
---
Monkey测试是Android平台自动化测试的一种手段，通过Monkey程序可以模拟用户触摸屏幕、滑动Trackball、按键等操作来对设备上的程序进行压力测试。

## 基础语法
基础语法如下：  
```
$ adb shell monkey [options]  
$ adb shell monkey -p your.package.name -v number  
$ monkey -p（Package的意思）指定包名 -v（Log级别）number（次数）
```
- 常用命令举例:   
> adb shell monkey -p com.example.administrator.aorise -v 100 > D:\monkey-log.txt  

- 普通的提测版本建议number设置十万次；上线版本建议做3*五十万次。

## 环境搭建  

1. [JDK下载](http://www.oracle.com/technetwork/java/javase/downloads/index.html)：需要设置Java环境变量。Java环境变量设置自行百度。  
![JDK](/assets/img/android-monkey/01.png)
2. ~~[Android 命令行工具](https://developer.android.com/studio/index.html)~~：需要设置Android环境变量。也可以不设置，直接在本地..\Android\sdk\platform-tools文件夹下打开cmd命令窗执行adb命令。  
![ADB Tools](/assets/img/android-monkey/02.png)

## LOG查看  

- 程序无响应的问题：在日志中搜索 “ANR”。
- 程序崩溃的问题：在日志中搜索 “Exception”“Fatal” (如出现空指针异常 NullPointerException，肯定是有bug）。

## 运行步骤  

> 1. 使用数据线连接Android设备和电脑（根据提示安装好Android设备驱动）。  
> 2. 在本地..\Android\sdk\platform-tools文件夹下打开cmd命令窗。  
> 3. ~~在cmd命令窗输入adb命令：adb devices 查看Android设备是否连接成功。~~  
> 4. 在cmd命令窗输入monkey命令：adb shell monkey [options]  
> 5. 按下回车[Enter]等待运行。  
> 6. 获取本地电脑上的LOG日志。  
