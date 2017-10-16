---
title: Web 性能指标 
layout: fasle
date: 2017-03-16 11:19:08
updated:
comments:
tags: 
  - Web 性能指标
categories:
  - performance
---

## 一. web 性能指标
根据 W3C Web Performance 工作组的[规范文档](https://w3c.github.io/perf-timing-primer/#)描述, 目前约定收集以下两种类型的性能数据。

**1）页面性能指标** 具体含义见第二点

```
	{
		"type": "navigationTiming",
		"url": "https://***",
		"data": {
		   "pageLoadTime": 8,  // 单位：ms
		   "redirectTime": 8,
		   "appCacheTime": 8,
		   "DNSQueryTime": 8,
		   "TCPConnTime": 8,
		   "requestTime": 8,
		   "domReadyTime": 8,
		   "pageRenderTime": 8
		}
	}
```

**2）资源性能指标** 具体含义见第三点

```
	{
		"type": "navigationTiming",
		"url": "https://***",
		"data": {
		   "resourceLoadTime": 8,  // 单位：ms
		   "redirectTime": 8,
		   "appCacheTime": 8,
		   "DNSQueryTime": 8,
		   "TCPConnTime": 8,
		   "requestTime": 8
		}
	}
		
```


## 二. 页面性能指标 

根据 W3C Web Performance 工作组的规范文档 [navigation-timing](https://w3c.github.io/navigation-timing/) 制定的页面整个处理过程如下

{% asset_img navigation-timing.svg "navigation-timing" %}


因此可约定页面性能指标JSON格式如下：

```
/*
	1. 整体关系公式：
		pageLoadTime = appCacheTime + dnsQueryTime + connectTime + requestTime + pageRenderTime
	2. TCP链接保持公用 或 使用缓存情况： fetchStart == connectEnd 
*/
{
	// pageLoadTime 页面加载总时间
	// 指从fetchStart事件开始到LoadEventEnd事件结束
	// 计算公式：timing.loadEventEnd - timing.fetchStart;
	"pageLoadTime": "",

	// redirectTime
	// 页面重定向的花费时间
   // 计算公式：timing.redirectEnd - timing.redirectStart
   "redirectTime": "",
   
	// appCacheTime 缓存时间
	// 指从fetchStart事件开始到解析DNS开始
   // 计算公式：timing.domainLookupStart - timing.fetchStart;
   "appCacheTime": "",
   
	// DNSQueryTime DNS查询时间
	// 指从解析DNS开始到完成
   // 计算公式：timing.domainLookupEnd - timing.domainLookupStart;
   "DNSQueryTime": "",
   
   // TCPConnTime TCP连接时间（包括https连接时间）
   // 指从开始三次握手到建立TCP链接完成
   // 计算公式：timing.connectEnd - timing.connectStart;
   "TCPConnTime": "",
   
   // requestTime 页面数据请求时间
   // 指页面数据的请求到响应接收完成时间
   // 计算公式：timing.responseEnd - timing.requestStart;
   "requestTime": ""
 
   // domReadyTime  DOM树创建完成时间
   // 指从responseEnd事件开始到domComplete事件
   // 计算公式：timing.domComplete - timing.responseEnd
   domReadyTime: "",
   
   // pageRenderTime 页面渲染时间
	// 指从responseEnd事件开始到loadEventEnd结束，包含DOM树创建和资源加载
	// 计算公式：timing.LoadEventEnd - timing.responseEnd
	pageRenderTime: ""
	
}
```


## 三. 资源性能指标

根据 W3C Web Performance 工作组的规范文档 [resource-timing](https://www.w3.org/TR/resource-timing-1/) 制定的页面整个处理过程如下

{% asset_img resource-timing.png "resource-timing" %}

这里的资源包括ajax（后台接口）, img, script, css, svg 等页面的所有下载资源。

因此可约定资源性能指标JSON格式如下：

```
/*
	1. 整体关系公式：
		resourceLoadTime = appCacheTime + dnsQueryTime + connectTime + requestTime
	2. TCP链接保持公用 或 使用缓存情况： fetchStart == connectEnd 
*/
{
	// resourceLoadTime 资源加载总时间
	// 指从fetchStart事件开始到资源响应接收完成结束
	// 计算公式：timing.responseEnd - timing.fetchStart;
	"resourceLoadTime": "",
	
	// redirectTime
	// 资源重定向的花费时间
   // 计算公式：timing.redirectEnd - timing.redirectStart
   "redirectTime": "",
   
	// appCacheTime 缓存时间
	// 指从fetchStart事件开始到解析DNS开始
   // 计算公式：timing.domainLookupStart - timing.fetchStart;
   "appCacheTime": "",
   
	// dnsQueryTime DNS解析时间
	// 指从解析DNS开始到完成
   // 计算公式：timing.domainLookupEnd - timing.domainLookupStart;
   "dnsQueryTime": "",
   
   // connectTime TCP连接时间（包括https连接时间）
   // 指从开始三次握手到建立TCP链接完成
   // 计算公式：timing.connectEnd - timing.connectStart;
   "connectTime": "",
   
   // requestTime 页面数据请求时间
   // 指页面数据的请求到响应接收完成时间
   // 计算公式：timing.responseEnd - timing.requestStart;
   "requestTime": ""
	
}
```


