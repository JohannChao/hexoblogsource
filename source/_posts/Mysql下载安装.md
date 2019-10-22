---
title: Mysql下载安装
tags: 
- mysql
categories:
- 环境配置
date: 2019-03-10 11:22:50
comments: true
---
### 下载

下载地址: [官网下载](https://dev.mysql.com/downloads/mysql/) 

下载合适的Mysql压缩包，本文为例，下载的版本是 "mysql-5.7.27-winx64.zip"
<!-- more -->

{% asset_img xiazaidizhi.png 下载地址 %}

下载完成后，将zip包解压到相应的目录下，本文解压目录是  "E:\softWare\mysql\mysql-5.7.27-winx64"

### 安装配置

打开解压目录，在该目录下创建 名为 my.ini 的配置文件

{% asset_img wenjiandizhi.png my.ini %}

文件内容为

```
[client]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=E:\\softWare\\mysql\\mysql-5.7.27-winx64
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
datadir=E:\\softWare\\mysql\\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
注意：文件分割符使用的是转义字符 "\\\\"

{% asset_img peizhiwenjian.png my.ini %}

以管理员身份打开cmd，切换目录至 ${mysql_home}\bin 目录下，执行下述命令
```
切换至此目录下：
E:\softWare\mysql\mysql-5.7.27-winx64\bin>

执行命令：
mysqld --initialize --console
```
执行成功后，会提示生成初始密码
```
...
2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
...
```
此密码用于后续登录mysql时使用。

再执行以下安装命令
```
mysqld install
```

初始化data目录
```
mysqld --initialize-insecure
```

启动mysql
```
net start mysql
```
至此mysql安装成功。

### 环境变量

{% asset_img cmd1.png 命令不识别 %}

我们每次在启动mysql 的时候，都需要先切换到 ${mysql_home}\bin 目录下，才会识别mysql命令。要使得每次打开cmd，都可以识别mysql命令，需要配置以下环境变量。

变量名 | 变量值
---|---
Path | E:\softWare\mysql\mysql-5.7.27-winx64\bin

配置完成后，识别mysql命令

{% asset_img cmd2.png 命令识别 %}

### 登录Mysql，修改密码

输入以下命令，登录mysql
```
mysql -h localhost -u root -p

-h 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略;

-u  登录的用户名;

-p : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。
```
然后输入上面默认生成的密码，即可登录mysql.

登陆成功后，可以使用以下命令，修改密码
```
mysql> set password for root@localhost = password('新密码');
```


### 可能遇到的问题

#### 由于找不到MSVCR120.dll,无法继续执行代码.重新安装程序可能会解决此问题

{% asset_img cuowu.png 找不到MSVCR120.dll %}

解决办法：这是由于缺少文件 vcredist ，[下载](https://www.microsoft.com/zh-CN/download/details.aspx?id=40784)安装这个文件即可。