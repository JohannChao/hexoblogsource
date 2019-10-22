---
title: IDEA中常见问题
date: 2019-10-11 09:59:37
tags: 
- IDE
- BUG
- java
categories:
- IDE
comments: true
---
在使用 IDEA 的过程中，遇到的一些问题，在此做一个总结，这些问题不都是IDE的，也有java的。

<!-- more -->

### 问题一
Cannot start compilation: the output path is not specified for module

在新建的类中，运行main方法，报错：
{% asset_img 11.png 图1.1 %}

**原因：**
这是由于没有设置Project的output路径导致的
{% asset_img 12.png 图1.2 %}

**解决：**
打开File---Project Structure
{% asset_img 13.png 图1.3 %}

在 output 下的空白框中，设置outPut路径，选择自己的Project，在路径后面拼接上\out即可。

### 问题二
java.lang.SecurityException: Prohibited package name: java.com.xxxx

{% asset_img 21.png 图2.1 %}

**原因：**

根据异常首要信息得知，是由于包名不合法所致。

点击报异常的位置，在ClassLoader包下的源码中，发现这段逻辑代码
{% asset_img 22.png 图2.2 %}

加载过程中，如果传入的类的包名是 null 或是以 "java." 开头，就会报出该异常。

**解决：**

修改包名即可。

在IDEA中，由于自己选择以 main 为源码包
{% asset_img 23.png 图2.3 %}
{% asset_img 24.png 图2.4 %}

现修改使用默认的 src 为源码包
{% asset_img 25.png 图2.5 %}
{% asset_img 26.png 图2.6 %}

### 问题三

Error:java: 无效的源发行版: 11

{% asset_img 31.png 图3.1 %}

**原因：**

使用的java版本不一致

**解决：**

可以修改Project中的 language level
{% asset_img 32.png 图3.2 %}

或者修改Module中的 language level
{% asset_img 33.png 图3.3 %}