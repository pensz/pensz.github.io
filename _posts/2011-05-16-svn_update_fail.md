---
layout: post
status: publish
published: true
title: svn update莫名失败的解决方法
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 62
wordpress_url: http://www.zwsun.com/?p=62
date: '2011-05-16 20:42:16 +0000'
date_gmt: '2011-05-16 12:42:16 +0000'
categories:
- "技术记录"
tags: []
---
<p>svn有时候会出现莫名 forbidden options，假设你的work copy是/svn/src/wc， 结果你update时，它会请求 /svn/src这个根目录，接着就是 forbidden 错误。这个问题在mac 和 win下我都遇见过，很是郁闷。</p>
<p>搜了半天，在万恶的资本主义国家找到点东西，说是svn的bug。</p>
<p>目前的解决方法就是对该地址重新 svn sw 一下。</p>
<p>&nbsp;</p>
<p>参考资料：</p>
<p>http://subversion.tigris.org/issues/show_bug.cgi?id=3242</p>
