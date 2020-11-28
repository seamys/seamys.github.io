---
layout: post
title: "在Windows 系统下安装llnode 和 lldb"
date: 2020-11-29 02:54:05
comments: true
categories: 
- 后端框架与技术 
tags: 
- nodejs
- llnode
- v8
---

## 简介

llnode 安装真的肝到我了. 安装的每一个步骤的开头都写着 “劝退”. 把这些安装步骤都记录下来吧. 方便后来人查看


## Windows 10 下安装 ubuntu 18.04 LTS

1. 首先去你的控制面板，在启用或者关闭Windows 功能里面，把适用于Linux 的Windows 子系统 启用了.

<img src="/static/image/1.enable-linux.png" title="enable-linux" width="400"/>

2. 在Windows store 里面搜索 “Ubuntu 18.04 LTS” 下载并安装

<img src="/static/image/2.install-ubuntu.png" title="install-ubuntu" width="400"/>

3. 打开 Ubuntu 命令行，进行安装和初始化，这里步骤就省略了，基本就是按照提示下一步了。 

## 给 APT 添加代理 

受制于国内的网络状况，给apt-get设置代理是非常有必要的操作，但是代理服务器我是没有办法提供给大家的。

1. 使用vi 打开 apt 的代理配置文件
``` bash
sudo vi /etc/apt/apt.conf.d/proxy.conf
```
2. 添加两条代理信息到里面 (引号里面的内容自行替换)
``` text
Acquire::http::Proxy "http://user:password@proxy.server:port/";
Acquire::https::Proxy "http://user:password@proxy.server:port/";
```
## 安装nodejs 和 npm
1. 添加 nodejs 源
```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```

2. 更新仓库
```bash
sudo apt-get update
```

3. 安装nodejs 和 npm
```bash
sudo apt-get install -y nodejs
```

4. 安装gcc g++ make. 方便安装各种c++ 扩展
```bash
sudo apt-get install gcc g++ make
```

5. 检查nodejs
```bash
nodejs -v
```
6. 检查npm
```bash
npm -v
```

## llnode 和 lldb 的安装

1. 安装 lldb 
```bash
sudo apt-get install lldb-4.0 liblldb-4.0-dev
```

2. 安装 llnode 4.0
```
sudo npm install --unsafe-perm --lldb_exe=`which lldb-4.0` -g llnode
```

以上就是所有的安装步骤。

## 参考链接：

[How to Install Node.js and npm on Ubuntu 18.04](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/)
[How to Set the Proxy for APT on Ubuntu 18.04](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-the-proxy-for-apt-for-ubuntu-18-04/)
[Github llnode](https://github.com/nodejs/llnode)
[sharp EACCES: permission denied on CentOS/RHEL 7 – FIXED](https://geekflare.com/sharp-eacces-permission-denied-fixed/)
