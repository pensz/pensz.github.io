---
layout: post
status: publish
published: true
title: swfupload笔记
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 249
wordpress_url: http://www.zwsun.com/?p=249
date: '2012-03-04 14:23:40 +0000'
date_gmt: '2012-03-04 06:23:40 +0000'
categories:
- "技术记录"
tags:
- swfupload
---
<p>很早以前记的，放在草稿箱里，没有发出来，现发出来，因为是笔记，所以看起来可能比较乱。</p>
<p>swfupload是上传大文件、显示上传进度以及一次上传多个文件的很好选择。但由于其采用了flash做为上传的客户端，带来了以下问题：</p>
<ul>
<li>cookie无法和浏览器共享：如果你限制用户登录了后才能上传，那么，用户在浏览器里的cookie你要通过post的形式传过去。</li>
<li>跨域问题：若swfupload所用的swf文件和上传处理段不在同一域名，便有此跨域问题，解决倒也简单，在上传处理端加个crossdomain.xml即可。</li>
</ul>
<p>crossdomain.xml示例：</p>
<blockquote>
<pre>&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;!DOCTYPE cross-domain-policy SYSTEM "http://www.adobe.com/xml/dtds/cross-domain-policy.dtd"&gt;
&lt;cross-domain-policy&gt;
    &lt;site-control permitted-cross-domain-policies="all"/&gt;
    &lt;allow-access-from domain="*.swfuploadfile.com" /&gt;
    &lt;allow-http-request-headers-from domain="*" headers="*" /&gt;
&lt;/cross-domain-policy&gt;</pre>
</blockquote>
<p>更让人头疼的是报2038错误（error=-200 message=#2038），这个错误实在折腾人啊。</p>
<p>先看看adobe官网是怎么说的：</p>
<p><a href="http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/FileReference.html?filter_flash=cs5&amp;filter_flashplayer=10.2&amp;filter_air=2.6" target="_blank">http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/FileReference.html?filter_flash=cs5&amp;filter_flashplayer=10.2&amp;filter_air=2.6</a><br />
里面讲到了ioerror出现的可能</p>
<ul>
<li>An input/output error occurs while the player is reading, writing, or transmitting the file.</li>
<li>The SWF file tries to upload a file to a server that requires authentication (such as a user name and password). During upload, Flash Player or Adobe AIR does not provide a means for users to enter passwords. If a SWF file tries to upload a file to a server that requires authentication, the upload fails.</li>
<li>The SWF file tries to download a file from a server that requires authentication, within the stand-alone or external player. During download, the stand-alone and external players do not provide a means for users to enter passwords. If a SWF file in these players tries to download a file from a server that requires authentication, the download fails. File download can succeed only in the ActiveX control, browser plug-in players, and the Adobe AIR runtime.</li>
<li>The value passed to the <code>url</code> parameter in the <code>upload()</code> method contains an invalid protocol. Valid protocols are HTTP and HTTPS.</li>
</ul>
<p><a href="http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/runtimeErrors.html" target="_blank">http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/runtimeErrors.html</a></p>
<ul>
<li><strong>2038</strong> File I/O Error.This error occurs when an application can't get file size, creation date or modification data using the FileReference API.</li>
</ul>
<p>大概意思就是没有办法获得文件信息，另外一方面是由于上传接口需要http验证，无法输入用户名和密码无法所致。</p>
<p>那么先检查一下上传接口是否有上述问题（使用最原始的上传方式检查），如果上传接口正常，那么需要想想是不是客户端的问题了。有用户说杀毒软件会禁止flash上传，这个我没有实际的验证，但的确遇到过在同一swfupload系统，在有些客户端报2038错，而在其他的地方正常。</p>
<p>若所有的客户端都出现此问题，那么还是有可能上传接口的问题，我就碰到一例：</p>
<p>情况如下：</p>
<ul>
<li>上传接口前面有一nginx做转发，且这个nginx距离实际处理上传文件的app服务器还有一段距离。</li>
<li>上传小文件正常，上传大文件会报2038 io error。</li>
<li>若不使用nginx转发，flash直接往app服务器上传文件，大文件也正常上传。</li>
</ul>
<p>去检查nginx的log</p>
<blockquote><p>cat nginx.log  | grep Flash | awk '$12 == 413 {print $0}'</p></blockquote>
<p>发现nginx的响应状态是499，而非正常的200。</p>
<p>怀疑nginx转发有问题，在客户端上传的同时，在nginx服务器上抓包看看：</p>
<blockquote><p>sudo tcpdump  -vvv -n -A host upload-app and port 80</p></blockquote>
<p>发现nginx转发的request的post数据不全，同时upload-app服务器上给出的响应是 400 bad request。</p>
<p>至此，初步将原因定为flash player主动断开连接，因为本地的nginx log里面是499，关于499，http协议rfc2616中没有定义，通过相关资料查询可得，499为客户端（flash player）主动断开连接。关于nginx的499可以参考以下文章：</p>
<ul>
<li><a href="http://forum.nginx.org/read.php?2,213789,213789" target="_blank">http://forum.nginx.org/read.php?2,213789,213789</a></li>
<li><a href="http://www.ruby-forum.com/topic/137312" target="_blank">http://www.ruby-forum.com/topic/137312</a></li>
</ul>
<p>为什么flash会主动断开连接？我也不知。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
