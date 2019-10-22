---
title: Java安装及环境配置
tags: 
- java
categories:
- 环境配置
date: 2019-03-10 09:14:23
comments: true
---
### 下载安装

#### 下载
使用以下网址，下载指定的JDK。

下载地址1：[官方下载](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

下载地址2： [jdk-8u221-windows-x64](https://pan.baidu.com/s/1vFoMuAYVQRgMeImJWe_rfg) 提取码[xrj9]
<!-- more -->

#### 安装
打开已下载的JDK，“下一步next”到底，中间可自定义自己的安装路径。
{% asset_img anzhuanglujing.png 安装路径 %}

### 配置环境变量

#### 完成环境变量的配置，需要添加以下几组环境变量。

变量名 | 变量值
---|---
JAVA_HOME | E:\softWare\java\jdk
CLASSPATH | .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;
Path | %JAVA_HOME%\bin

{% asset_img hjbl1.png 环境变量配置 %}

{% asset_img hjbl2.png 环境变量配置 %}

### 验证安装

打开CMD，输入 java -version 或 javac -version

{% asset_img cmd.png 安装验证 %}

若有输出，则证明安装成功。