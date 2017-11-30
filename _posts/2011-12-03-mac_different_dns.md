---
layout: post
status: publish
published: true
title: mac中针对不同域名使用不同的dns服务器
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 240
wordpress_url: http://www.zwsun.com/?p=240
date: '2011-12-03 12:51:24 +0000'
date_gmt: '2011-12-03 04:51:24 +0000'
categories:
- "技术记录"
- mac os
tags:
- dns
---
<p>一般情况下，我们使用一个dns服务器就能达到我们的需求，但有时候我们需要针对不同域名，使用不同的dns，如以下场景：</p>
<ul>
<li>需要dns服务器A去解释某国外域名</li>
<li>需要dns服务器B去解释局域网内部的域名</li>
<li>默认使用dns服务器C</li>
</ul>
<p>通用的解决方法是使用自己搭建的dns forwarder服务器，然后针对不同的zone设置不同的dns服务器，bind和dnsmasq都可以做这种dns forwarder的工作。</p>
<p>但如果你是在mac中，则有更加简单的方法：</p>
<ol>
<li>在 /etc/ 目录下建立 resolver 文件夹，即 /etc/resolver 目录</li>
<li>在/etc/resolver 目录下，针对不同的域名zone建立不同的解析文件；如我要对zwsun.com使用192.168.1.1的dns服务器，则建立文件zwsun.com,内容为： nameserver 192.168.1.1</li>
<li>ping zwsun.com看看，证实一下是否设置成功</li>
</ol>
<p>上述操作需要在root下，另外，需要说明的 是，nslookup的结果不会受这个文件的影响，但对ping程序是会生效的。</p>
<p>一般的策略是将大部分域名解析的dns设置在 /etc/resolv.conf中，也就是通过网络偏好设置，特殊解析需求的可以在/etc/resolver目录下建立对应的文件。</p>
