---
title: "HTTP: The Definitive Guide"
layout: post
date: 2015-11-22
image: ""
tags:
- http
categories:
- web
---

## HTTP概述

### 连接

#### TCP/IP

- 无差错的数据传输
- 按序传输
- 未分段的数据流(任意时刻任意尺寸发送数据)

### Web结构组件

#### 代理

- 接受所有客户端HTTP请求，并将这些请求转发给服务器

#### 缓存

#### 网关

![gateway](/assets/image/http-guide/http-ch01-1.png)

#### 隧道

在一条或者多条HTTP连接上转发非HTTP数据，转发时不会窥探数据。

#### Agent代理

Web浏览器，spiders

## URL与资源

URL语法：

```html
<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>
```

## HTTP报文

报文由`start line`, `header`, `body`组成，分类：`request message`和`response message`:

请求报文格式：

```html
<method> <request-URL> <version>
<headers>

<entity-body>
```

响应报文：

```html
<version> <status> <reason-phrase>
<headers>

<entity-body>
```

### 状态码：

### 首部：

#### 通用首部：

![http-03-shared-headers](/assets/image/http-guide/http-03-shared-headers.png)

#### 通用缓存首部

![http-03-shared-cache-headers](/assets/image/http-guide/http-03-shared-cache-headers.png)

#### 请求-信息性首部

![http-03-request-info-headers](/assets/image/http-guide/http-03-request-info-headers.png)

#### 请求-Accept首部

![http-03-request-accept-headers](/assets/image/http-guide/http-03-request-accept-headers.png)

#### 请求-条件请求首部

![http-03-request-condition-headers](/assets/image/http-guide/http-03-request-condition-headers.png)

#### 请求-安全请求首部

![http-03-request-safe-headers](/assets/image/http-guide/http-03-request-safe-headers.png)

#### 请求-代理请求首部

![http-03-request-proxy-headers](/assets/image/http-guide/http-03-request-proxy-headers.png)

#### 响应-信息首部

![http-03-response-info-headers](/assets/image/http-guide/http-03-response-info-headers.png)

#### 响应-协商首部

![http-03-response-negotiation-headers](/assets/image/http-guide/http-03-response-negotiation-headers.png)

#### 响应-安全响应首部

![http-03-response-safe-headers](/assets/image/http-guide/http-03-response-safe-headers.png)

#### 实体-信息首部

![http-03-content-info-headers.png](/assets/image/http-guide/http-03-content-info-headers.png)

#### 实体-内容首部

![http-03-content-body-headers.png](/assets/image/http-guide/http-03-content-body-headers.png)

#### 实体-缓存首部

![http-03-content-cache-headers.png](/assets/image/http-guide/http-03-content-cache-headers.png)

## 连接管理

TCP收到HTTP数据流，将数据流砍成被称作段的小数据块，并将段封装在IP分组中。一个IP分组包括：

- IP分组首部(20字节)
- TCP段首部(20字节)
- TCP数据块(0个或多个字节)

![04-ip-group.png](/assets/image/http-guide/04-ip-group.png)

对TCP性能的考虑:

最常见的TCP相关时延：

- TCP连接建立握手
- TCP慢启动拥塞控制
- 数据聚集的Nagle算法
- 用于捎带确认的TCP延迟确认算法
- TIME_WAIT时延和端口耗尽

### TIME_WAIT时延和端口耗尽

当某个TCP端点关闭TCP连接时，会在内存中维护一个小的控制块，用来记录最近关闭连接的IP地址和端口号。这类信息会维持一段时间，通常是估计的最大分段使用期的两倍(称为2MSL,通常2分钟)，**以确保在这段时间内不会创建具有相同地址和端口号的新连接**

**2MSL的连接关闭延迟通常不是什么问题，但是在性能基准(测试服务器)环境下就可能会成为一个问题**

`<soucr-IP-address, source-port, destination-IP-address, destination-port>` 这几个只有源端口号可以随意改变。

### HTTP连接的处理

串行事务处理时延:

![04-serial-transaction-processing.png](/assets/image/http-guide/04-serial-transaction-processing.png)

