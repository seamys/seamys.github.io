---
layout: post
title: "CsQuery .Net下的jQuery实现"
date: 2016-03-17 23:06:05
comments: true
categories: 
- 后端开发与技术
tags: 
- CsQuery
- C#
- jQuery
---

> [CsQuery Github](https://github.com/jamietre/CsQuery) 作者已经不再积极维护此项目。


## CsQuery 简介

CsQuery实现了所有CSS2和CSS3选择器。实现jQuery的DOM操作方法和工具方法。jQuery 1.6.2 的多数测试已经在C#上成功运行。

## 为什么需要 CsQuery?

jQuery使用CSS选择器非常容易在客户端上操作HTML。没有理由在服务端使用更复杂的方式来处理这样的事情。它在eb项目上做HTML页面后处理，比如网页抓取后做页面分析等。 

### 符合标准的HTML解析

CsQuery使用validator.nu HTML解析器的C＃实现和Gecko 浏览器引擎使用相同的代码。CsQuery将创建从同一来源任何基于Gecko的浏览器相同的DOM。你应该想到它处理有效和无效的标记的优异的成绩。

### CSS3选择器和jQuery方法

CsQuery实现了所有CSS2和CSS3选择器和过滤器，以及全面的DOM模型。您可以使用所有相同的jQuery你熟悉遍历和操纵DOM方法。

### 快速索引CSS选择器

该CSS选择器引擎完全索引Html文档上的tagName，ID，class和属性。该指数是子选择能力，这意味着复杂的选择仍然能够采取指数（用于编入索引选择的任何部分）的优势。选择的性能相对于其他现有的C＃HTML解析库是快几个数量级。
更重要的是，从灒（jQuery的CSS选择器引擎）和jQuery整个测试套件（1.6.2）已经从Javascript移植到C＃来弥补这一项目。

### 难以置信的简单

CQ 对象包含你所有需要的功能，CQ对象可以像jQuery一样使用，你可以把字符串赋值到CQ 对象上进行解析。通过 `[...]` 来运行jQuery选择器，并返回新的CQ对象。像 jQuery中 `$('..')`一样。最后通过`Render`返回 DOM 字符串。使用CQ 你可以完整的使用jQuery API 遍历和操作HTML文档。
下面有一些HTML解析，选择，修改和操作后返回字符串的例子。

``` csharp

CQ dom = "<div>Hello world! <b>I am feeling bold!</b> What about <b>you?</b></div>";


CQ bold = dom["b"];               /// find all "b" nodes (在例子中有两个)
       
  > bold.ToList()
  > Count = 2
  > [0]: {<b>...</b>}
  > [1]: {<b>...</b>}

  > bold.First().RenderSelection()
  > "<b>I am feeling bold!</b>"

string boldText = bold.Text();        /// jQuery text 方法;

  > boldText
  > "I am feeling bold! you?"

bold.Remove();                        /// jQuery remove 方法
string html = dom.Render();           

  > html
  > "<div>Hello world!  What about </div>"

```

### 资源列表

CsQuery Github： [https://github.com/jamietre/CsQuery](https://github.com/jamietre/CsQuery) 
