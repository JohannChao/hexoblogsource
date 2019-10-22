---
title: Tomcat安装，配置
tags: 
- java
categories:
- 环境配置
date: 2019-03-10 10:35:41
comments: true
---
### 下载安装

下载地址： [官方下载](https://tomcat.apache.org/download-80.cgi)

下载合适的压缩包，下载完成对其解压，即可完成安装。
<!-- more -->
{% asset_img xiazaidizhi.png 下载地址 %}

### 环境变量

变量名 | 变量值
---|---
CATALINA_BASE | E:\softWare\tomcat\apache-tomcat-8.5.43
CATALINA_HOME | E:\softWare\tomcat\apache-tomcat-8.5.43
Path | %CATALINA_HOME%\lib
Path | %CATALINA_HOME%\bin

{% asset_img hjbl1.png 环境变量 %}

{% asset_img hjbl2.png 环境变量 %}

### 启动/关闭 tomcat

#### 启动方式1

进入安装目录下的bin目录，双击 startup.bat 即可完成启动。

{% asset_img qd1.png 启动方式 %}

启动成功后，访问 localhost:8080

{% asset_img qd2.png 访问tomcat %}

#### 启动方式2

进入命令行，输入 startup 命令  [startup.bat命令 也可以]，即可启动 tomcat。

{% asset_img qd3.png 命令行启动 %}

#### 关闭方式1

进入bin目录下，双击 shutdown.bat 即可关闭 tomcat。

进入命令行，输入 shutdown.bat ，也可以关闭 tomcat。Windows下，如果输入的命令是 shutdown , 则会和windows本身的shutdown命令冲突。

{% asset_img gb.png 命令冲突 %}

### 启动乱码的问题

在启动tomcat的过程中，控制台有乱码的情况出现：

{% asset_img luanma1.png 启动乱码 %}

解决办法：修改 E:\softWare\tomcat\apache-tomcat-8.5.43\conf 下的 logging.properties 文件

{% codeblock %}
java.util.logging.ConsoleHandler.encoding = UTF-8
改为
java.util.logging.ConsoleHandler.encoding = GBK
{% endcodeblock %}

{% asset_img luanma2.png 修改配置 %}

修改完成后，启动不再乱码

{% asset_img luanma3.png 启动不再乱码 %}
