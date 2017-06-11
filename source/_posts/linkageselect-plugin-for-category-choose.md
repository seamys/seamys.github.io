---
layout: post
title: JQuery 无限级下拉选择插件
date: 2016-03-16 23:06:05
categories: 
- 前端开发
tags: 
- jQuery插件
- javascript
- html
- select
---

# levelSelector

https://seamys.github.io/levelSelector/example/used-in-complex-data-formats.html

插件主要应对无限级树型结构的数据。比如说国家、省、市、区、县级市、村、街道的选择，还有一些B2B之类的网站无限极分类。
无论本地数据还是异步获取这些在后端系统对内容进行归类是常常用到。`jQuery.levelSelector.js` 就是为了解决此问题的。

![](https://images2015.cnblogs.com/blog/329473/201603/329473-20160324114449058-1763367235.gif)

## 快速开始
下载[压缩版本][min] 或者 [开发版本][max].

[min]: /dist/jquery.levelSelector.min.js
[max]: /dist/jquery.levelSelector.js

### 1. 依赖文件
``` html
<!--引入jquery 1.7 以上的版本-->
<script src="jquery.js"></script>
<!--引入插件-->
<script src="jquery.levelSelector.js"></script>
```
### 2. 数据脚本格式

我们简单使用一些数据来填充使用（并非一定需要使用当前格式后面详细介绍使用）。

``` javascipt
var data = {
     id_0: [{ Id: 1, Name: '服装' }, { Id: 2, Name: '鞋包' }],
     id_1: [{ Id: 3, Name: '男装' }, { Id: 4, Name: '女装' }],
     id_2: [{ Id: 5, Name: '男鞋' }, { Id: 6, Name: '女鞋' }, { Id: 7, Name: '箱包' }],
     id_3: [{ Id: 8, Name: '男式上装' }, { Id: 9, Name: '男式裤子' }],
     id_4: [{ Id: 7, Name: '女式裙子' }, { Id: 8, Name: '女式裤子' }]
}
```
数据格式，`id_0` 节点是所有节点的根分类。根分类下有 `服装`、`鞋包`。服装的 `Id`为 1 则服装的子分类`key`为 id_1，服装下有 `男装`、`女装`。以此类推。
### 3. 调用插件
``` html
<!-- 用一个div 存储结果-->
<div id="show"></div>
<div class="levelSelector"></div>
<script type="text/javascript">
    $(".levelSelector").levelSelector({
        //select 中任意值修改后触发此回调方法 
        change: function (v) {
           $("#show").text(v.join());
         },
         //数据提供回调方法
         provider: function (params, callback) {
             var id = params[params.length - 1];
             callback(data["id_" + id]);
         }
     });
</script>
```
### 4. 查看demo

[点击查看 DEMO](https://seamys.github.io/levelSelector/example/getting-started.html)

![image](https://images2015.cnblogs.com/blog/329473/201603/329473-20160324114331042-385521810.gif)

## 使用 bower 安装
``` sh
 bower install levelSelector
```
## 参数说明


*`url(string)`* : 在异步工作下，插件通过此URL获取相应分类子分类

*`handel(function)`* : 创建select内option时将调用此方法,当您无法控制服务器或者数据源格式的时可以重写此方法。

*`path(string)`* : 在页面初始化时,指定此参数可以初始化选中的select值。例如：`0,2,38,65`,

*`sChar(string)`* : 用于指定拆分 path的字符。 默认 "`,`"

*`create(function)`* : 创建 `select` 时调用，回调方法必须返回一个jQuery select 对象。

*`filter(function)`* : 方法在 provider方法执行后调用，负责对数据进行二次过滤。

*`param(function)`* : 如果你无法控制服务器提供的参数名称时可以调用此方法重写服务器参数。

*`provider(function)`*: 数据提供者，您可以通过此方法来重写数据提供的方式。

### 默认参数如下
``` javascipt
		{
            url: "/levelSelector/list",
            handel: function (v) {
                return "<option value=" + v.Id + ">" + v.Name + "</option>";
            },
            path: 0,
            sChar: ',',
            change: function (arrays) { return arrays; },
            create: function () {
                return $("<select>");
            },
            filter: function (arrays) {
                return arrays;
            },
            param: function (arrays) {
                return { ids: arrays.join(defaults.sChar) };
            },
            provider: function (arrays, callback) {
                $.ajax(defaults.url, {
                    type: 'GET',
                    dataType: "json",
                    cache: true,
                    data: defaults.param(arrays || [0]),
                    success: callback
                });
            },
            defOption: '请选择..'
        }
```
## 开发文档


## Examples

https://seamys.github.io/levelSelector/example/

## Release History

2016年1月30日 0.1.0 Release

