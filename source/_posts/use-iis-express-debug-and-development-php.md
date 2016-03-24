---
layout: post
title: "使用 IIS Express 调试和开发PHP"
date: 2014-05-04 23:06:05
categories: 
- 后端框架与技术 
tags: 
- IIS
- PHP
---

## 简介

使用 IIS Express 调试和开发PHP的好处是多多的。如果你是dotnet程序员又因为又包含开发PHP开发的相关项目。可以通过使用IIS Express 来同时调试你的.net 程序和php 程序。
IIS Express 在安装vs2012 版本以上的IDE已经自带了。
关于 IIS Express 相关资料请点击

 - [IIS Express简介](http://msdn.microsoft.com/zh-cn/ff844168)
 - [IIS Express – Getting Started Tutorial](http://www.winserverhelp.com/2010/07/iis-express-getting-started/)


## 配置方法

1.修改applicationhost.config文件（C:\Users\<你的用户名>\Documents\IISExpress\config）。

```xml
<defaultDocument enabled="true">
 <files>
  <add value="Default.htm" />
  <add value="Default.asp" />
  <add value="index.htm" />
  <add value="index.html" />
  <add value="iisstart.htm" />
  <add value="default.aspx" />
  <!–-添加一项-–>
  <add value="index.php" />
 </files>
</defaultDocument>
```

2.找到fastcgi节点添加下列信息。

```xml
<fastCgi>
<application fullPath="C:phpphp-cgi.exe" monitorChangesTo="php.ini" activityTimeout="600"
requestTimeout="600" instanceMaxRequests="10000">
<environmentVariables>
<environmentVariable name="PHP_FCGI_MAX_REQUESTS" value="10000" />
<environmentVariable name="PHPRC" value="C:php" />
</environmentVariables>
</application>
</fastCgi>
```
`注意：这里的“C:\phpphp-cgi.exe”是我的电脑里的安装目录。实际请修改你自己的安装目录`

3.在<handlers accessPolicy="Read,Script">下面添加下列信息。

```xml
<add name="PHP52_via_FastCGI" path="*.php" verb="GET,HEAD,POST" modules="FastCgiModule" scriptProcessor="C:\phpphp-cgi.exe" resourceType="Either" />
```
4.运行调试

IIS Express 启动只能手动启动。可以使用命令行cmd 打开命令行，然后执行

```bash
"C:\Program Files\IIS Express" /siteid:7
``` 

`注意：/siteid:7 是你配置中给的id 你可以将上述命令保持为 .bat 文件方便调试。`
