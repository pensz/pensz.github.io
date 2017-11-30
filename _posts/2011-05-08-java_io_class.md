---
layout: post
status: publish
published: true
title: java io类小结
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 59
wordpress_url: http://www.zwsun.com/?p=59
date: '2011-05-08 15:16:21 +0000'
date_gmt: '2011-05-08 07:16:21 +0000'
categories:
- "技术记录"
tags:
- java
---
<ol>
<li>以 Writer Reader 结尾的类是text file相关的，既然与text file相关，那么必然有charset的概念, 暂存数组就是char[]；</li>
<li>以 InputStream OutputStream 结尾的类都是binary file相关的，与charset无关，暂存数组就是byte[]；</li>
<li>以 Buffered类开头的类都是用来做buffer用的，不考虑效率问题，可以从来不使用这些类；</li>
<li>以 File开头的类都是与File相关的，其他的场景，想都不用想了，都写文件，先想想File开头的类；</li>
<li> PrintWriter 比 FileWriter更加通用。</li>
</ol>
