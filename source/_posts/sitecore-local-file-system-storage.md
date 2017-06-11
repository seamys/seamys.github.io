---

title: Sitecore 缓存本地存储实现
date: 2017-06-24 00:25:12

categories: 
- 后端开发
tags:
- sitecore
- github
---


## 1.	简介
Sitecore 本地调试开发启动预热需要加载缓存远程数据库item数据。 Sitecore 7.5 使用的是缓存使用的是Hashtable 实现的，并且没有提供给开发任何可以扩展的机会。
 
这种缓存实现方式，意味着每次调试，启动，发布或者回收应用程序池，会导致已经缓存到内存里的数据丢失.

说明文档提供能将Sitecore每次查询结果缓存到本地磁盘中的方法。
## 2.	如何使用
### 2.1.	下载类库
代码托管在Github:
https://github.com/seamys/Sitecore.Data.LocalDataProvider
使用Git 下载:
Git clone https://github.com/seamys/Sitecore.Data.LocalDataProvider.git
Zip包下载下载:
https://github.com/seamys/Sitecore.Data.LocalDataProvider/archive/sitecore7.5.zip
 
### 2.2.	编译项目
a)	打开项目文件夹，复制 Sitecore.Kernel.dll 到项目 ”\libs\7.5“ 文件夹中

b)	VS打开项目并编译生成：Sitecore.Data.LocalDataProvider.dll

c)	将编译的Sitecore.Data.LocalDataProvider.dll复制到Web bin目录下

### 2.3.	修改Web.config dataApi, dataProviders节点配置
修改dataApi节点 configuration-> sitecore -> dataApis->dataApi:

``` xml
<dataApi name="SqlServer" type="Sitecore.Data.LocalDataProvider.$(database)LocalDataApi, Sitecore.Data.LocalDataProvider">
     <param connectionStringName="$(1)" />
</dataApi>

```

修改dataProviders 节点 configuration-> sitecore -> dataProviders ->main:

``` xml
<main type="Sitecore.Data.LocalDataProvider.$(database)LocalDataProvider, Sitecore.Data.LocalDataProvider">
	<param name="api" ref="dataApis/dataApi" param1="$(1)"/>
   <Name>$(1)</Name>
 </main>

```
### 2.4.	刷新页面查询并缓存数据
数据扩展会将sitecore每次查询的数据存储到磁盘中。如果缓存从Hashtable 丢失相同的查询会直接从硬盘中读取。
## 3.	缓存查看工具
项目文件中包含 ” Sitecore.Data.LocalDataProvider.Tools“ 缓存查看工具。

项目编译完毕后 打开 Sitecore.Data.LocalDataProvider.Tools.exe 

 

拖放Cache文件到软件Query Result 区域中

 
## 4.	特殊配置
项目文件中 ”Sitecore.LocalDataProvider.config.example“ 可以对缓存进行指定配置实例。
将文件移除 .example 后缀。 放置到 “\App_Config\Include” 文件夹中（非必须）

参数说明：
是否开启磁盘缓存（默认：true）：
``` xml
<setting name="LocalData.Enable" value="false"/>
```
指定需要缓存的数据库（默认值：*）:
``` xml
<settingname="LocalData.Allow.Databases" value="Sitecore_web,Sitecore_master"/>
```

需要过滤掉的Query (默认值：EventQueue)
``` xml
<setting name="LocalData.Exclude.Querys" value="EventQueue"/>
```
## 5.	缓存文件共享
如果缓存结束后可以把缓存后的缓存文件压缩发送给其他开发使用。避免从新生成
