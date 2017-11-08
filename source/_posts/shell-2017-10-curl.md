---
title: curl 模拟 Http 的get or post请求
date: 2017-10-16 09:56:12
tags:
    - curl
categories:
    - shell
---

## 一、Get 请求 

```
curl "http://www.baidu.com"     # 请求资源
curl -i "http://www.baidu.com"  # 并显示全部信息
curl -l "http://www.baidu.com"  # 只显示头部信息
curl -v "http://www.baidu.com"  # 显示get请求全过程解析

```

## 二、Post 请求
```
curl -d '{"deviceId":"bc4b6c446338f1deeff4bc26606b4af4"}' https://mzone.yqb.com/mzone-http/index                          # Post 请求
curl -H "Content-Type:application/json"    # 并指定请求头
curl --cookie "name=xxx" www.example.com   # 并指定 cookies
```

### 设置代理
```
curl -x 127.0.0.1:8888                     # 并制定代理
```


