---
layout: post
title: HTTP协议相关笔记
date: 2016-08-03 18:06:24.000000000 +08:00
tags: http
---

### 非常常见的状态码

首先是罗列一些学习工作中出现几率很高的状态码

- 200 OK
- 304 Not Modified 未修改
- 400 Bad Request 错误的请求
- 401 Unauthorized 未授权
- 403 Forbidden 拒绝访问
- 404 Not Found 未找到
- 500 Internal Server Error 内部服务器错误

- 501 Not Implemented 未执行
- 502 Bad Gateway 错误的网关
- 503 Service Unavailable 服务不可用
- 504 Gateway Timeout 网关超时


### 更多内容

#### 标准扩展码

1xx Informational 信息化

100 Continue 继续

101 Switching Protocols 交换协议

102 Processing 处理

2xx Success 成功

200 OK

201 Created 创建

202 Accepted 已接受

203 Non-Authoritative Information 非授权信息

204 No Content 无内容

205 Reset Content 重置内容

206 Partial Content 部分内容

207 Multi-Status 多状态

208 Already Reported 已报告

226 IMIM Used 使用的

3xx Redirection 重定向

300 Multiple Choices 多种选择

301 Moved Permanently 永久移动

302 Found 发现

303 See Other 查看其它

304 Not Modified 未修改

305 Use Proxy 使用代理

306 Switch Proxy 开关代理

307 Temporary Redirect 临时重定向

308 Permanent Redirect 永久重定向

4xx Client Error 客户端错误

400 Bad Request 错误的请求

401 Unauthorized 未授权

402 Payment Required 需要付费

403Forbidden 拒绝访问

404 Not Found 未找到

405 Method Not Allowed 不允许的方法

406 Not Acceptable 不可接受

407 Proxy Authentication Required 代理服务器需要身份验证

408 Request Timeout 请求超时

409 Conflict 冲突

410 Gone 完成

411 Length Required 需要长度

412 Precondition Failed 前提条件失败

413 Payload Too Large 负载过大

414 URI Too Long 太长

415 Unsupported Media Type 不支持的媒体类型

416 Range Not Satisfiable 的范围不合适

417 Expectation Failed 预期失败

418 I'm a teapot 我是一个茶壶

421 Misdirected Request 误导请求

422 Unprocessable Entity 无法处理的实体

423 Locked 锁定

424 Failed Dependency 失败的依赖

426 Upgrade Required 升级所需

428 Precondition Required 所需的先决条件

429 Too Many Requests 太多的请求

431 Request Header Fields Too Large 请求头字段太大

451 Unavailable For Legal Reasons 不可出于法律原因

5xx Server Error 服务器错误

500 Internal Server Error 内部服务器错误

501 Not Implemented 未执行

502 Bad Gateway 错误的网关

503 Service Unavailable 服务不可用

504 Gateway Timeout 网关超时

505 HTTP Version Not Supported 不支持HTTP版本

506 Variant Also Negotiates 变体也进行协商

507 Insufficient Storage 存储空间不足

508 Loop Detected 检测到循环

510 Not Extended 不延长

511 Network Authentication Required 网络需要身份验证

