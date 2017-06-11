---
layout: post
title: "如何使 WebAPI 自动生成漂亮又实用在线API文档"
date: 2016-3-22 13:35:12
categories: 
- 后端开发与技术
tags: 
- C#
- SwaggerUI
- Swashbuckle
- CsQuery
---

## 1.前言

### 1.1 [SwaggerUI](http://swagger.io/swagger-ui/) 
[SwaggerUI](http://swagger.io/swagger-ui/) 是一个简单的Restful API 测试和文档工具。简单、漂亮、易用（[官方demo](http://petstore.swagger.io/)）。通过读取JSON 配置显示API. 项目本身仅仅也只依赖一些 html,css.js静态文件. 你可以几乎放在任何Web容器上使用。
### 1.2 [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) 
[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) 是.NET类库,可以将WebAPI所有开放的控制器方法生成对应[SwaggerUI](http://swagger.io/swagger-ui/)的JSON配置。再通过[SwaggerUI](http://swagger.io/swagger-ui/) 显示出来。类库中已经包含[SwaggerUI](http://swagger.io/swagger-ui/) 。所以不需要额外安装。

## 2.快速开始
 - 创建项目 **OnlineAPI**来封装**百度音乐服务**([示例下载](https://files.cnblogs.com/files/Arrays/OnlineAPI.rar)) ，通过API可以搜索、获取音乐的信息和播放连接。

 我尽量删除一些我们demo中不会用到的一些文件，使其看上去比较简洁。

<img width="800" alt="Swashbuckle配置文件" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322154604761-736833879.png">

 - WebAPI 安装 [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)
``` csharp
Install-Package Swashbuckle
```
 - 代码注释生成文档说明。
[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) 是通过生成的XML文件来读取注释的，生成 [SwaggerUI](http://swagger.io/swagger-ui/)，JSON 配置中的说明的。
安装时会在项目目录 App_Start 文件夹下生成一个 SwaggerConfig.cs 配置文件，用于配置 [SwaggerUI](http://swagger.io/swagger-ui/) 相关展示行为的。如图：

<img width="800" alt="Swashbuckle配置文件" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322154638386-80214618.png">

- 将配置文件大概99行注释去掉并修改为

``` csharp
c.IncludeXmlComments(GetXmlCommentsPath(thisAssembly.GetName().Name));
```

- 并在当前类中添加一个方法
``` csharp
/// <summary>
/// </summary>
/// <param name="name"></param>
/// <returns></returns>
protected static string GetXmlCommentsPath(string name)
{
     return string.Format(@"{0}\bin\{1}.XML", AppDomain.CurrentDomain.BaseDirectory, name);
}
```
- 紧接着你在此Web项目属性生成选卡中勾选 “XML 文档文件”，编译过程中生成类库的注释文件

<img width="800" alt="生成XML" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322154705886-1132698503.png">

- 添加百度音乐 3个API

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322154731698-1275900571.png">

- 访问 `https://<youhost>/swagger/ui/index`,最终显示效果

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322155022245-428689097.png">

- 我们通过API 测试API 是否成功运行

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322155040511-1048217314.png">

## 3.添加自定义HTTP Header
在开发移动端 API时常常需要验证权限，验证参数放在Http请求头中是再好不过了。WebAPI配合过滤器验证权限即可

首先我们需要创建一个 IOperationFilter 接口的类。IOperationFilter

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Http;
using System.Web.Http.Description;
using System.Web.Http.Filters;
using Swashbuckle.Swagger;

namespace OnlineAPI.Utility
{
    public class HttpHeaderFilter : IOperationFilter
    {
        public void Apply(Operation operation, SchemaRegistry schemaRegistry, ApiDescription apiDescription)
        {
            if (operation.parameters == null) operation.parameters = new List<Parameter>();
            var filterPipeline = apiDescription.ActionDescriptor.GetFilterPipeline();
            //判断是否添加权限过滤器
            var isAuthorized = filterPipeline.Select(filterInfo => filterInfo.Instance).Any(filter => filter is IAuthorizationFilter);
            //判断是否允许匿名方法
            var allowAnonymous = apiDescription.ActionDescriptor.GetCustomAttributes<AllowAnonymousAttribute>().Any();
            if (isAuthorized && !allowAnonymous)
            {
                operation.parameters.Add(new Parameter
                {
                    name = "access-key",
                    @in = "header",
                    description = "用户访问Key",
                    required = false,
                    type = "string"
                });
            }
        }
    }
}
```
在 SwaggerConfig.cs 的 EnableSwagger 配置匿名方法类添加一行注册代码

``` csharp
c.OperationFilter<HttpHeaderFilter>();

```

添加Web权限过滤器

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Text;
using System.Web;
using System.Web.Http;
using System.Web.Http.Controllers;
using Newtonsoft.Json;

namespace OnlineAPI.Utility
{
    /// <summary>
    /// 
    /// </summary>
    public class AccessKeyAttribute : AuthorizeAttribute
    {
        /// <summary>
        /// 权限验证
        /// </summary>
        /// <param name="actionContext"></param>
        /// <returns></returns>
        protected override bool IsAuthorized(HttpActionContext actionContext)
        {
            var request = actionContext.Request;
            if (request.Headers.Contains("access-key"))
            {
                var accessKey = request.Headers.GetValues("access-key").SingleOrDefault();
                //TODO 验证Key
                return accessKey == "123456789";
            }
            return false;
        }

        /// <summary>
        ///     处理未授权的请求
        /// </summary>
        /// <param name="actionContext"></param>
        protected override void HandleUnauthorizedRequest(HttpActionContext actionContext)
        {
            var content = JsonConvert.SerializeObject(new {State = HttpStatusCode.Unauthorized});
            actionContext.Response = new HttpResponseMessage
            {
                Content = new StringContent(content, Encoding.UTF8, "application/json"),
                StatusCode = HttpStatusCode.Unauthorized
            };
        }
    }
}
```

在你想要的ApiController 或者是 Action 添加过滤器

```
[AccessKey]
```

最终显示效果

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322155111604-218029648.png">

## 4.显示上传文件参数
SwaggerUI 有上传文件的功能和添加自定义HTTP Header 做法类似，只是我们通过特殊的设置来标示API具有上传文件的功能


``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Http.Description;
using Swashbuckle.Swagger;

namespace OnlineAPI.Utility
{
    /// <summary>
    /// 
    /// </summary>
    public class UploadFilter : IOperationFilter
    {

        /// <summary>
        ///     文件上传
        /// </summary>
        /// <param name="operation"></param>
        /// <param name="schemaRegistry"></param>
        /// <param name="apiDescription"></param>
        public void Apply(Operation operation, SchemaRegistry schemaRegistry, ApiDescription apiDescription)
        {
            if (!string.IsNullOrWhiteSpace(operation.summary) && operation.summary.Contains("upload"))
            {
                operation.consumes.Add("application/form-data");
                operation.parameters.Add(new Parameter
                {
                    name = "file",
                    @in = "formData",
                    required = true,
                    type = "file"
                });
            }
        }
    }
}
```
在 SwaggerConfig.cs 的 EnableSwagger 配置匿名方法类添加一行注册代码

``` csharp
c.OperationFilter<UploadFilter>();

```

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322155134136-1251422400.png">

API 文档展示效果

<img width="800" alt="WebAPI" src="https://images2015.cnblogs.com/blog/329473/201603/329473-20160322155150651-1690324017.png">

## 5.版本和资源
你可以通过下列连接获取相关说明。
OnlineAPI Demo
[https://files.cnblogs.com/files/Arrays/OnlineAPI.rar](https://files.cnblogs.com/files/Arrays/OnlineAPI.rar)
Swashbuckle 项目地址：
[https://github.com/domaindrivendev/Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)
swagger-ui  项目地址：
[https://github.com/swagger-api/swagger-ui](https://github.com/swagger-api/swagger-ui)
swagger-ui 官网地址：
[http://swagger.io/swagger-ui/](http://swagger.io/swagger-ui/)