---
layout: post
title: "科学上网概述"
date: 2019-01-05 10:37
comments: true
tags: 
	- 笔记
---

### 科学上网
#### 现行上网方式，GFW原理
GFW，即Great Fire Wall, 长城防火墙的简称，是中国政府过滤和监控互联网的一套软硬件系统。GFW的作用主要是用于分析和过滤中国境内外网络间的互相访问。也就是说，他不仅能限制国内网民访问境外的某些站点，也能限制国外用户访问国内的站点。  我们通常说的“被墙”，就是指访问被GFW所限制。而”翻墙“，顾名思义，则是突破GFW的限制。

GFW的工作原理一般被视为国家机密，普遍的看法是通过DNS劫持和IP黑名单等多种机制
1. 关键词过滤阻断，主要是针对HTTP的80端口，GFW在数据流中发现了特定的关键词后，会阻断网络。在任何阻断发生后，一般在随后的90s内，都无法访问对应IP地址相同端口上的内容
2. IP地址封锁， 设立IP黑名单，所有路由到该IP的请求都不予转发
3. 特定端口封锁，特定IP上的特定端口， 如SSH的22、VPN的1723、SSL的443，类似IP地址封锁

<!--more-->
4. DNS劫持和污染，GFW主要采用DNS劫持和污染技术，使用Cisco提供的IDS系统来进行域名劫持，防止访问被过滤的网站，2002年Google被封锁期间其域名就被劫持到百度。中国部分ISP也会通过此技术插入广告。 

参考链接：[GFW的工作原理](https://blog.csdn.net/u010884939/article/details/40129339)，[GFW的实现原理和技术（墙与翻墙）](https://www.cnblogs.com/weicyNo-1/p/8125763.html)，[防火长城-维基百科](http://zh.wiki.blackshao.com/wiki/%E9%98%B2%E7%81%AB%E9%95%BF%E5%9F%8E)

#### 流行的科学上网方法
- VPN
虚拟私人网络（英语：Virtual Private Network，缩写为VPN）是一种常用于连接中、大型企业或团体与团体间的私人网络的通讯方法。它利用隧道协议（Tunneling Protocol）来达到保密、发送端认证、消息准确性等私人消息安全效果，这种技术可以用不安全的网络（例如：互联网）来发送可靠、安全的消息。
[VPN-维基百科](http://zh.wiki.blackshao.com/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)，
[比较VPN协议: PPTP 对 L2TP 对 OpenVPN 对SSTP 对 IKEv2](https://zh.vpnmentor.com/blog/%E6%AF%94%E8%BE%83vpn%E5%8D%8F%E8%AE%AE-pptp-%E5%AF%B9-l2tp-%E5%AF%B9-openvpn-%E5%AF%B9sstp-%E5%AF%B9-ikev2/)
[VPN 隧道协议PPTP、L2TP、IPSec和SSLVPN的区别](https://linux.cn/article-3407-1.html)

- SS
Shadowsocks缩写ss 是轻量级的开源的服务器中转包传输工具。该软件在中国大陆一般被当作 socks5 代理服务。当前包使用Python、C、C++、C#、Go语言等编程语言开发，大部分主要实现（iOS平台的除外）采用Apache许可证、GPL、MIT许可证等多种自由软件许可协议开放源代码。
[shadowsocks的github项目源码](https://github.com/search?q=shadowsocks)
[shadowsocks-维基百科](http://zh.wiki.blackshao.com/wiki/Shadowsocks)
[shadowsocks官网-已被屏蔽](https://shadowsocks.org/en/index.html)

- v2ray
V2Ray是Project V 的核心工具，其主要负责网络协议和功能的实现，与其它 Project V 通信。V2Ray 可以单独运行，也可以和其它工具配合，以提供简便的操作流程。
主要特性： 多入口多出口，一个V2Ray进程可以并发多个入站和出站协议；可定制化路由。可实现按区或按域名分流；多协议支持，包括Socks、HTTP、Shadowsocks、VMess等等；隐蔽性，支持混淆，避免第三方干扰；反向代理；多平台支持
[v2ray官方网站-已被屏蔽](https://www.v2ray.com/)

#### 自建SS服务器
1. 购买vps（虚拟个人服务器）
推荐搬瓦工，有钱的话可以去亚马逊或是其他一些云服务厂商
对延迟要求较高的话，建议优先选择香港日本或者新加坡的服务器，正常ping延迟在100ms左右，美国的话，不是CN2的线路，延迟在200-300ms左右，不是用来玩游戏这类对延迟要求较高的活动的话，影响不大
<img src="/picture/banwagong-vps.png" width="50%" height="50%" tips="搬瓦工VPS配置">
2. 服务端搭建
[Linux安装shadowsocks服务端方法](https://blog.csdn.net/finishx/article/details/79039362)
3. 端口密码配置
可以配置多个，多客户端同时使用不影响
```
{
    "server":"my-ssserver",
    "local_address":"127.0.0.1",
    "local_port":8888,
    "port_password":{
         "9999":"password0",
         "9001":"password1",
         "9002":"password2",
         "9003":"password3",
         "9004":"password4"
    },
    "timeout":600,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
4. 客户端配置
[shadowsocks官网](https://shadowsocks.org/en/download/clients.html)提供各种平台的客户端，找到对应平台，下载客户端
<img src="/picture/shadowsocks-client.png" width="70%" height="70%">
或者直接去github上下载发布包，并配置好服务器即可链接，尝试访问google，可以即表示成功


#### PC端解决方案
无论是Windows还是macOS，在github上都有长期维护的客户端源码和发布包可以使用，安装包小巧轻便无广告。
<img src="/picture/ss_config.png" width="50%" height="50%">

#### 手机端解决方案
1. 安卓端-SS客户端
google play 应用商店下载shadowsocks
或者github上搜索 shadowsocks 找到安卓版本的下载发布包，[shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android/releases)
<img src="/picture/WechatIMG13.jpeg" width="30%" height="30%">

2. ios端-VPN
ios比较特殊，有Shadowrocket等客户端app，但是国区没有开放下载，各种山寨客户端大行其道，数据账号安全没法保证。
推荐在VPS上部署VPN服务，在手机端用vpn实现科学上网
[针对IOS设备科学上网的IKEv2的服务安装和客户端使用](https://quericy.me/blog/699/)

#### VPS还可以用来干嘛
- 搭建个人博客
1. 域名选择购买和使用—— [goddady](https://godaddy.com)
2. 博客网站框架选择—— [hexo官网](https://hexo.io/zh-cn/docs/index.html)
3. nginx

- 利用nginx反向代理境外网站
代理维基百科：
```
server {
      server_name  ~^(?<subdomain>[^.]+)\.wiki\.blackshao\.com$;
      listen 80;
      resolver 8.8.8.8;
        location /search-redirect.php {
        proxy_pass https://$subdomain.wikipedia.org;
        proxy_buffering off;

        proxy_redirect https://$subdomain.wikipedia.org/ http://$subdomain.wiki.blackshao.com/;
        proxy_redirect https://$subdomain.m.wikipedia.org/ http://$subdomain.m.wiki.blackshao.com/;
        proxy_cookie_domain $subdomain.wikipedia.org $subdomain.wiki.blackshao.com;

        proxy_set_header X-Real_IP $remote_addr;
        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header Accept-Encoding '';
        proxy_set_header referer "https://$proxy_host$request_uri";

        subs_filter_types text/css text/xml text/javascript application/javascript application/json;
        subs_filter .wikipedia.org .wiki.blackshao.com;
        subs_filter //wikipedia.org //wiki.blackshao.com;
        subs_filter 'https://([^.]+).wiki' 'http://$1.wiki' igr;
        subs_filter upload.wikimedia.org up.wiki.blackshao.com;
        more_set_headers -s '302' "Location: http://zh.wiki.blackshao.com/wiki/$arg_search";
      }
```
- 搭建个人网盘、学习linux等等