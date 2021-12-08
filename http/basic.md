# 浅析 URL

![hello world!](images/url.jpg)

## 李爵士发明的三样东西

WWW = URL + HTTP + HTML

## URL

URL = 协议 + 域名或 IP + 端口号 + 路径 + 查询字符串 + 锚点

## 协议
使用 http 或 https 等协议，完成从客户端到服务器端等一系列运作流程。

## IP

IP（Internet Protocol）主要约定了两件事：如何定位一台设备和如何封装数据报文，以跟其他设备交流。

## 域名

域名就是对 IP 的别称。一个域名可以对应不同的 IP，这个叫做均衡负载，防止一台机器扛不住。一个 IP 可以对应不同域名，这个叫做共享主机，穷开发者会这么做。

## 域名的级别
域名分为顶级域名、二级域名（俗称顶级域名）、三级域名（俗称二级域名）。比如：`xiedaimala.com` 在这个域名中，`com` 为顶级域名，`xiedaimala.com` 是二级域名，`wwww.xiedaimala.com` 为三级域名。


## 几个特殊的 IP

- `127.0.0.1` 表示自己
- `localhost` 通过 hosts 指定为自己
- `0.0.0.0` 不表示任何设备

## DNS
DNS（Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。他提供域名到 IP 地址之间的解析服务。

## 端口

一台机器可以提供很多服务，每个服务一个号码，这个号码就叫端口号 port。

一共有 65535 个端口，常见端口如下：

- 80 端口：提供 HTTP 服务
- 443 端口：提供 HTTPS 服务
- 21 端口：提供 FTP 服务

## 端口使用规则

0~1023 号端口是留给系统使用的，你只有拥有了管理员权限后，才能使用这 1024 个端口。其他端口可以给普通用户使用，比如 http-server 默认使用 8080 端口。一个端口如果被占用你就只能换另一个端口。


## 路径

如何请求不同的页面？路径可以做到。

```
https://developer.mozilla.org/zh-CN/docs/Web/HTML
```

## 查询字符串 

如何请求同一个页面的不同内容？查询参数可以做到。

```
www.baidu.com/s?wd=hi
```

## 锚点

如何请求同一个页面同一个内容的不同位置？锚点可以做到。

```
https://developer.mozilla.org/zh-CN/docs/Web/CSS#参考书
```

锚点看起来有中文，实际不支持中文，`#参考书` 会变成 `#%E5%8F%82%E8%80%83%E4%B9%A6`。锚点是无法在 Network 面板看到的，因为锚点不会传给服务器。



## nslookup

使用 nslookup 命令可以查询给定域名的 IP 地址，比如：

```
nslookup wikipedia.com
Server:         209.222.18.222
Address:        209.222.18.222#53

Non-authoritative answer:
Name:   wikipedia.com
Address:    208.80.154.224
```

## ping
使用 ping 命令可以测试主机在互联网协议（IP）网络上的可访问性。比如：

```
ping wikipedia.com
```