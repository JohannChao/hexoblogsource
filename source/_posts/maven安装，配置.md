---
title: maven安装，配置
tags: 
- maven
categories:
- 环境配置
date: 2019-03-10 10:05:12
comments: true
---
### 下载安装

下载地址：[官网下载](http://maven.apache.org/download.cgi)

下载完成后，解压到指定的文件夹，即可完成安装。
<!-- more -->

### 环境变量


变量名 | 变量值
---|---
MAVEN_HOME | E:\softWare\maven\apache-maven-3.6.1
Path | %MAVEN_HOME%\bin
MAVEN_OPTS [可选配置] | -Xms128m -Xmx512m

{% asset_img hjbl2.png 环境变量 %}

{% asset_img hjbl1.png 环境变量 %}

### 验证安装

运行命令 mvn -v

输入内容，则说明安装成功。

{% asset_img banbenyanzheng.png 安装验证 %}

### 可选配置

#### 1，修改maven本地仓库

如果没用改动，使用maven的过程中，下载的jar包，默认的保存路径是 "${user.home}/.m2/repository"。我们可以通过，修改配置文件，指定下载路径。

跳转到maven安装路径下，修改conf文件夹下的 settings.xml 文件。

{% codeblock %}
<localRepository>E:\softWare\maven\repository</localRepository>
{% endcodeblock %}

{% asset_img bendiku.png maven本地库修改 %}

修改完成后，之后的所有jar包将下载到此路径下。

#### 2，修改maven远程仓库

还是修改安装目录下的settings.xml文件。

{% codeblock %}
<mirror>  
     <id>alimaven</id>  
     <name>aliyun maven</name>  
     <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
     <mirrorOf>central</mirrorOf>          
</mirror>
{% endcodeblock %}

{% asset_img yuanchengku.png maven远程仓库修改 %}

