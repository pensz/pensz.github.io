---
layout: post
status: publish
published: true
title: mac os中使用windows共享文件和打印机
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 55
wordpress_url: http://www.zwsun.com/?p=55
date: '2011-05-08 13:45:45 +0000'
date_gmt: '2011-05-08 05:45:45 +0000'
categories:
- mac os
tags:
- mac os
- "共享"
- "打印机"
---
<p>办公室里面现在一般还是用的windows的机器，打印机，有时候我们需要从mac os中访问windows共享的资源，主要有共享文件和打印机。方法如下：</p>
<p>1 访问windows共享文件：打开 Finder &gt; 前往 &gt; 连接服务器，输入smb://ip地址，根据提示输入用户名，密码和需要访问的目录即可挂载。不推荐使用Finder窗口左侧的网络访问。</p>
<p>2 共享打印机：打开 系统偏好设置 &gt; 打印与传真 ， 点击左侧的 + ，添加打印机，选择windows tab（windows共享的打印机，别选IP 那个tab），选择共享的机器hostname和对应的打印机即可，一般的打印机，系统都有相关驱动。添加后即可打印。</p>
