---
layout: post
title: "Shadowsocks 自搭梯子完全教程"
date: 2017-10-7 5:10:00
comments: true
categories: 
- 工具
tags: 
- Shadowsocks
---

## 1. 简介
这次国庆节用了两年的服务商终于趴了,连着2天是没有链接上. 每年国庆节的时候似乎都能死一批服务商.
究其原因还是树大招风,用的人多了就容易被封.

晚上憋不住, 2个小时不到梯子就搭好了. 开始写这个博客的时候时间已经是凌晨 2.06. 好了现在开始介绍如何用最快的方式搭建一个的 [Shadowsocks](https://zh.wikipedia.org/wiki/Shadowsocks) 服务.

## 2. 购买VPS
首先你需要购买一个VPS服务,淘宝搜索VPS会有很多.  我用的是 [linode](https://zh.wikipedia.org/wiki/Linode).
购买Linode VPS 有两种方式,一种是直接在[官方网站](https://www.linode.com/)上购买,另一种方式你通过国内的代理渠道购买. 官方购买需要提供信用卡(master,visa). 下面介绍代理商注册购买方式.

访问链接 https://cp.ivpser.com/linode (这里我省略注册相关流程)

**选择机房:日本东京. 操作系统选择 Ubuntu 系统**

<img src="/static/image/vps-1.jpg" title="hlbt1" width="800"/>

**最便宜的应该40块钱每个月 1TB的流量(足够你使用了).**

<img src="/static/image/vps-2.jpg" title="hlbt1" width="800"/>

**下一步选择需要购买多少月, 然后支付. 稍等一两分钟你会收到一份邮件. 邮件内就有关于服务器信息**

## 3. 配置服务器
购买服务器后因为服务器使用的是默认密码,服务器无疑是处于裸奔状态.如果你仅仅是用于翻墙用途的建议做下面的操作.
登陆 Linode 后台面板(https://manager.linode.com)

**1.重新初始化服务器,重置 root 密码**

<img src="/static/image/rebuild.jpg" title="hlbt1" width="800"/>

**2.使用网页客户端 卸载 ssh 服务端(以后就使用网页客户端).**
登陆控制台
<img src="/static/image/console.jpg" title="hlbt1" width="800"/>
卸载 openssh-server
```bash
apt-get remove --auto-remove openssh-server

```

## 4. 安装 Shadowsocks 代理服务
安装 Shadowsocks 服务端非常容易,几行命令就能搞定. [官方文档](https://github.com/shadowsocks/shadowsocks/blob/master/README.md)
**更新安装包列表**
```bash
apt-get update
```
安装Python软件包索引
```bash
apt-get install python-pip
```
安装git工具(这样可以直接安装GitHub上最新的版本)
```bash
apt-get install git
```
安装shadowsocks服务端

```bash
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

测试一些是否安装成功
```bash
ssserver --version
```
如果安装成功,控制台会打印如下信息
```bash
Shadowsocks 3.0.0
```
## 5. 配置并启动代理服务
创建一个服务配置文件
```bash
nano /etc/shadowsocks.json
```
在文件内添加如下内容

```javascript
{
    "server":"<这里填写你的服务器IP>",
    "server_port":443,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"<这里填写你的密码>",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false,
    "workers":5
}
```
关闭并保存文件.启动代理服务
```bash
ssserver -c /etc/shadowsocks.json -d start
```
如果你看到如下信息
```bash
INFO: loading config from /etc/shadowsocks.json
2017-10-06 20:07:33 INFO     loading libcrypto from libcrypto.so.1.0.0
started
```
说明你服务已经正常启动. 

现在你需要进一步确认你的电脑能否链接这台服务器的端口.
在你本机打开 cmd 控制台. 使用telnet尝试链接一下代理服务器
```bash
telnet <这里是你服务器的IP> 443
```
如果能够正常链接上,说明服务器这边已经完全搭建好了.
## 6. Windows客户端使用
shadowsocks-windows 也是shadowsocks 整个项目的一部分. 
下载列表
https://github.com/shadowsocks/shadowsocks-windows/releases
选择最新的版本下载包(Latest release-> Downloads).
如果一起顺利你得到一个客户端软件
配置运行

<img src="/static/image/windows-gui.jpg" title="hlbt1" width="400"/>


