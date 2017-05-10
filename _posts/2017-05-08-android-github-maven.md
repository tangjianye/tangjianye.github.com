---
layout: post  
title:  利用Github创建maven仓库  
date:   2017-05-08 
tags: [Android]  
categories: Android  
author: JayaTang  
description: 利用Github托管功能创建代码maven仓库。
---
利用Github托管功能创建代码maven仓库。

## 环境
- Github账号。
- Github上托管maven仓库的工程AoriseMaven（命名可以随意）。
- 本地编译生成pom文件的源码工程。

## 远程仓库
1. 利用Github创建用于托管pom文件的空仓库AoriseMaven.
2. 克隆空仓库到本地：'D:\Github\AoriseMaven'.

## 源码工程
1. 在需要打包托管到maven仓库的**module工程**的'build.gradle'文件添加如下配置：  
 
  ```Gradle
  apply plugin: 'com.android.library'

  android {
    ...
  }

  dependencies {
    ...
  }

  apply plugin: 'maven'
  apply from: '../config/maven-common.gradle'
  ```
2. 在工程**根目录**的'gradle.properties'文件添加编译生成的pom文件的output路径：  
  
  ```Gradle
  aar.deployPathCommon=D\:\\GitHub\\AoriseMaven
  ```
3. 在工程**根目录**创建'config/maven-common.gradle'文件：  

  ```Gradle
  ext {
      PUBLISH_GROUP_ID = 'cn.aorise'
      PUBLISH_ARTIFACT_ID = 'common'
      PUBLISH_VERSION = android.defaultConfig.versionName
  }

  uploadArchives {
      repositories.mavenDeployer {
          def deployPath = file(getProperty('aar.deployPathCommon'))
          repository(url: "file://${deployPath.absolutePath}")
          pom.project {
              groupId project.PUBLISH_GROUP_ID
              artifactId project.PUBLISH_ARTIFACT_ID
              version project.PUBLISH_VERSION
          }
      }
  }
  ```
4. 在需要打包托管到maven仓库的**module工程**根目录执行gradle命令：  

  ```Gradle
  gradle uploadArchives
  ```

## 上传POM文件
1. Gradle命令成功后会在配置的output路径(D:\Github\AoriseMaven)生成POM文件，文件结构如下：
![POM文件包结构](/assets/img/android-github-maven/01.png)
![POM文件结构](/assets/img/android-github-maven/02.png)
2. Push文件到Github远程仓库。
3. Github的maven远程仓库的地址如下：
> https://raw.githubusercontent.com/github的username/仓库名/master

## 应用Maven仓库
1. 在调用maven仓库的AS工程文件根目录的'build.gradle'文件添加如下配置：
```Gradle
allprojects {
    repositories {
        ...
        // aorise commonLib 远程仓库地址
        maven { url "https://raw.githubusercontent.com/tangjianye/AoriseMaven/master" }
    }
}
```
2. 在调用maven仓库的module的'build.gradle'文件添加如下配置：
```Gradle
dependencies {
  ...
  compile 'cn.aorise:common:1.0.3'
}
```

以上步骤全部完成后，module模块就可以调用maven远程仓库里面的内容，包括函数、资源文件、assets里面的文件等。