---
title: HTTP 缓存
date: 2017-12-07 11:07:31
categories: HTTP
tags: HTTP
---

缓存可归为两类：私有与共享缓存。共享缓存存储的响应能够被多个用户使用。私有缓存只能用于单独用户。

<!-- more -->

## 缓存控制
定义 `cache-control` 头来区分对缓存机制的支持情况，请求头和响应头都支持这个属性。通过它提供的不同的值来定义缓存策略。
1. 禁止进行缓存
```
cache-control: no-store
cache-control: no-cache, no-store, must-revalidate
```
2. 强制确认缓存
缓存会将此请求发到服务器（该请求应该会带有与本地缓存相关的验证字段），服务器会验证请求中所描述的缓存是否过期，若未过期（实际就是返回304），则缓存才使用本地缓存副本。
```
cache-control: no-cache
```
3. 私有缓存和公共缓存
```
cache-control: private
cache-control: public
```
4. 缓存过期机制
`max-age` 是距离请求发起的时间的秒数。
```
cache-control: max-age=300
```
5. 缓存验证确认
```
cache-control: must-revalidate
``` 

**Pragma** 头，请求中包含 `pragma` 的效果跟在头信息中定义 `cache-control: no-cache` 相同，但是 HTTP 的响应头不支持这个属性，通常定义 `pragma` 以向后兼容基于 HTTP/1.0 的客户端。

## 新鲜度
当客户端发起一个请求时，缓存检索到已有一个对应的陈旧资源，则缓存会先将此请求附加一个 `If-None-Match` 头，然后发给目标服务器，以此来检查该资源副本是否是依然还算新鲜的。

## 缓存验证
