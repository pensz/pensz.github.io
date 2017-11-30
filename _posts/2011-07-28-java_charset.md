---
layout: post
status: publish
published: true
title: java字符集小记
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 88
wordpress_url: http://www.zwsun.com/?p=88
date: '2011-07-28 21:35:29 +0000'
date_gmt: '2011-07-28 13:35:29 +0000'
categories:
- "技术记录"
tags:
- java
- servlet
- "乱码"
- "字符集"
- "编码"
---
<p>该blog整理自我的笔记，可能比较乱。</p>
<p>1 String.getBytes(String charset) 方法：是将string 以 charset来编码，同一个string 可以以很多不同的charset来编码，得到的Byte数组也是不一样的。</p>
<div>2 new String (byte[], String charset) 这个是将byte数组里面的数据以charset来解码，这里很容易出现乱码，原因就是charset选择不对。</div>
<div></div>
<div>3 为什么会存在 以下代码转码的情形：</div>
<div>
<blockquote><p>String utf8 = new String (a.getBytes("8859_1"), "gb2312");</p></blockquote>
</div>
<div>
<p>原因是：首先 a 从人类角度来看是乱码了； 再次，a之前是以8859_1来编码的，后来发现真实的编码应该是 gb2312。其中的 a.getBytes("8859_1")只是为了获得最初的byte[]，如果你有最初的byte[]，完全不需要通过a.getBytes("8859_1")来获得。</p>
</div>
<div>
<blockquote><p>String a = new String (bo.toByteArray(), "8859_1");</p></blockquote>
</div>
<div>
<blockquote><p>String correct = new String (a.getBytes("8859_1"), "gb2312");</p></blockquote>
</div>
<div>
<p>上面这段代码等于</p>
</div>
<div>
<blockquote><p>String correct = new String (bo.toByteArray(), "gb2312");</p></blockquote>
</div>
<p>4 java 里面的string是没有charset一说的，都是以unicode来保存的。</p>
<div>5 关于servlet，jsp里面的乱码问题</div>
<div>5.1 request.setCharacterEncoding(String) 是对request 的 body的解码有用，故设置这个对解决GET参数里面的乱码没有用；l另外需要注意浏览器发送请求的原始编码是否正确</div>
<div>5.2 response.setCharacterEncoding(String) 声明一下编码即可保证输出正确的编码；需要注意的是：必须要在getWriter前设置。详情可以参考 javadoc：</div>
<blockquote>
<div>This method can be called repeatedly to change the character  encoding.  This method has no effect if it is called after  <code>getWriter</code> has been  called or after the response has been committed.</div>
</blockquote>
<div>6 如何检测编码？</div>
<div>检测编码就是计算这一组字节在各个字符集上解码的可读性，选择可读性最高的那个编码为最终的编码，所以这件事情也不是那么简单的。</div>
<div>当然，utf8 bom时会有一些特殊byte可以判断。</div>
<div>对于html来说，简单的方法是看http header和html中声明的编码。</div>
