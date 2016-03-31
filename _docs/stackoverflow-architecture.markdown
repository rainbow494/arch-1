---
author: Aaron Zhang
home: https://github.com/aaronz
layout: default
title: StackOverflow架构简介
date: 2016-3-28 00:00:00
---

StackOverflow是一个程序员最爱的问答网站。它由两个以前顶尖程序员、知名博主[Jeff Atwood](http://www.codinghorror.com/blog/)和[Joel Spolsky](http://www.joelonsoftware.com/)创建。某种程度上像是一个名人拥有的饭店，有可能会红一阵子。但Joel估计全世界有1/3的程序员都在使用这个网站，所以他们的网站一定是有不错的内容。  

![StackOverflow](/assets/images/posts/stackoverflow-logo.png)

<!--more-->

## The Stats

- 1600万 PV/月
- 300万 UV/月 (当时facebook 77 million UV/月)
- 600万 访问/月
- 86% 流量来自Google
- 全球900万活跃程序员其中30%用过StackOverflow.
- StackOverflow通过微软的[BizSpark](http://www.microsoft.com/BizSpark)计划获得了更优惠的价格。他们只需要付11K美金为OS和SQL授权。
- 扩展策略：不扎眼的广告，招聘广告，DevDays大会，扩展应用到其他相关领域(server fault, super user)，开发了一套程序员评级系统。

## Platform

*   Microsoft ASP.NET MVC
*   SQL Server 2008
*   C#
*   Visual Studio 2008 Team Suite
*   JQuery*   LINQ to SQL
*   Subversion
*   Beyond Compare 3
*   VisualSVN 1.5
*   Web Tier  

- 2 x Lenovo ThinkServer RS110 1U  
- 4 cores, 2.83 Ghz, 12 MB L2 cache  
- 500 GB datacenter hard drives, mirrored  
- 8 GB RAM  
- 500 GB RAID 1 mirror array*   Database Tier  
- 1 x Lenovo ThinkServer RD120 2U  
- 8 cores, 2.5 Ghz, 24 MB L2 cache  
- 48 GB RAM


*   第四台机器加入来运行[superuser.com](http://superuser.com). 同时也运行[stackoverflow](http://stackoverflow.com),[Server Fault](http://serverfault.com/), 和[Super User](http://superuser.com/).
*   使用QNAP TS-409U NAS做备份. 不使用云的原因在于需要5G的带宽太烧钱。
*   主机host在 http://www.peakinternet.com/. 技术支持和服务价格都很到位.
*   SQL Server的全文搜索被广泛的应用，如检测一个问题是否被问过。同时Lucene.net也被作为一个不错的备选方案。  

## 经验之谈

这些经验来自于Jeff和Joel两个人，包括一些源自他们博文评论中的总结。

* 如果你对服务器管理不排斥的话那就直接买下来。租用服务器两个问题在于
	- 夸张的内容和硬盘升级费用
	- 服务提供商根本就不会管理


