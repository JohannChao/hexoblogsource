---
title: 一篇文章搞定nvm，node.js，npm，nrm安装
tags: 
- Node
categories:
- 环境配置
date: 2019-03-10 14:31:44
comments: true
---
### 入门

#### nvm，node.js，npm，nrm的关系
{% codeblock %}1, nvm ： nvm是node.js的版本管理工具，用于管理node.js和npm的版本。
2, node.js ： node.js是javascript的一种运行环境，是对Google V8引擎进行的封装，是一个服务器端的javascript的解释器。
3, npm ： npm是随同node.js一起安装的包管理工具，npm管理对应nodeJs的第三方插件。
4, nrm ： nrm是npm registry管理工具，可以用来管理npm的镜像源。
{% endcodeblock %} 
<!-- more -->

{% asset_img guanxitu.png nvm，node.js，npm关系图 %}

#### 为什么要用nvm来管理node版本

在开发中，有时候对node的版本有要求，有时候需要切换到指定的node版本来重现问题等。遇到这种需求的时候，我们需要能够灵活的切换node版本。

#### nrm有什么用

当我们使用npm来下载所需要的包时，可能由于网络的原因，需要等待很长时间才能完成下载，使用nrm切换npm镜像源为taobao，可以快速下载完成。

{% asset_img nrmyongchu.png nrm管理npm镜像源 %}

---

### Windows下nvm的下载安装

由于官方版本并不支持Windows，所以需要 [nvm-windows](https://github.com/coreybutler/nvm-windows/releases)来实现Windows下的 nvm 安装。

{% codeblock %} 下载之前，请确保已卸载所有的Node.js
{% endcodeblock %} 

[说明文档](https://github.com/coreybutler/nvm-windows/wiki)
#### 下载免安装包

{% asset_img nvmxiazai.png nvm下载地址 %}

解压完成后，有如下几个文件

{% asset_img nvmjieya.png nvm解压 %}

#### 环境变量

解压完成后，还需要设置以下环境变量


变量名 | 变量值
---|---
NVM_HOME | E:\softWare\nvm
NVM_SYMLINK | E:\softWare\nodejs
Path | %NVM_HOME%
Path | %NVM_SYMLINK%

{% codeblock %}NVM_HOME ： nvm的解压路径
NVM_SYMLINK ： 路径用于标识使用的哪个版本的node.js
{% endcodeblock %} 

{% asset_img hjbl.png nvm环境变量 %}

#### 新建 settings.txt

在解压目录下，新建 settings.txt 文件，文件包含以下属性

{% codeblock %}
root ： 存储不同版本node.js的路径，同 NVM_HOME;
path ： 当前使用的node.js的路径，同NVM_SYMLINK；
proxy ： 一般设置为 "none"。如果需要使用代理，可以使用命令行中的nvm对其进行修改。
arch ： 标识操作系统，32或64
{% endcodeblock %}

{% asset_img setting.png setting.txt示例 %}

#### nvm安装完成

使用 nvm 命令验证，是否安装成功。

{% asset_img nvmcmd.png nvm验证 %}

---

### 使用 nvm 安装node.js

#### 切换nvm镜像源

nvm默认的下载地址是 [nvm默认镜像源](http://nodejs.org/dist/) ，国内访问的话会很慢。
所以在使用nvm安装node之前，建议切换镜像源。

切换nvm镜像源的方法很简单，只需要把下面两行，复制到上面的 settings.txt 文件中即可。

{% codeblock %}
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
{% endcodeblock %}

{% asset_img nvmjingxiang.png nvm镜像源 %}

#### 安装node

{% codeblock %}
常用命令:
nvm list available #查看远程所有可用的node版本
nvm list #查看本地所有node版本
nvm install 8.11.2 #安装 8.11.2 版本
nvm use 8.11.2 #切换至 8.11.2 版本
nvm uninstall 8.11.2 #卸载8.11.2 版本
{% endcodeblock %}

选择一个版本下载，下载 node.js 的同时，会把npm也一同下载下来。

下载完成后，通过 node -v 和 npm -v 命令，查询当前的版本。

{% asset_img node_npm.png node，npm版本 %}

---

### nrm -- NPM registry manager

当使用 npm 下载第三方插件的时候，默认使用的镜像源会请求国外服务器，国内访问的话，会比较慢，通过使用 nrm ，可以在不同的 npm 镜像源之间快速切换。

#### 安装nrm

{% codeblock %}
npm install -g nrm
{% endcodeblock %}

#### nrm的使用

{% codeblock %}
使用 nrm -ls 命令，可以查看所有可切换的 npm 镜像源。其中，带 * 的为当前正在使用的 npm 镜像源。

执行 nrm use <name> 命令，用于切换 npm 镜像源。
{% endcodeblock %}

{% asset_img npmjingxiang.png nrm管理npm镜像源 %}

切换完成后，之后再使用 npm install 下载第三方插件时，都会从新的镜像源地址进行下载。

