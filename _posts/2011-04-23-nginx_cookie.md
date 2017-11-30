---
layout: post
status: publish
published: true
title: "从nginx的cookie中获得用户访问的时间"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 42
wordpress_url: http://www.zwsun.com/?p=42
date: '2011-04-23 23:38:36 +0000'
date_gmt: '2011-04-23 15:38:36 +0000'
categories:
- "技术记录"
tags: []
---
<p>由于需要跟踪用户行为，在nginx服务端使用user模块开启了<a href="http://wiki.nginx.org/HttpUserIdModule">nginx的cookie</a>，使用服务器来设置cookie而不是通过程序来设置的好处在于：</p>
<blockquote><p>1 比较简单，不需要自己写程序维护</p>
<p>2 图片等静态文件也可以设置cookie</p></blockquote>
<p>本想通过分析log获得用户第一次入站的时间，但通过观察nginx的log，发现它设置的cookie有以下规律：</p>
<blockquote><p>1 32位的sid，第一个8位和第三个8位的值相同，最后一个8位的值是等差数列</p>
<p>2 第二个8位的后4位相同，前面4位不同</p></blockquote>
<p>nginx sid样例：</p>
<blockquote><p>9000A8C0 6E15 AD4D 734A84B1 02180303<br />
9000A8C0 4616 AD4D 734A84B1 02190303<br />
9000A8C0 D616 AD4D 734A84B1 021A0303</p></blockquote>
<p>鉴于有这么明显的规律，我认定其中必有私货。上IRC问了一下，暂时没人回答，于是决定自己看看源代码。</p>
<p>按图索骥，根据nginx wiki 上列的模块关系，一下可以找到user对应的代码为：</p>
<p>svn cat svn://svn.nginx.org/nginx/trunk/src/http/modules/ngx_http_userid_filter_module.c</p>
<p>nginx的代码非常清楚，代码看起来也比较易读,在534行左右可以看到uid的各个元素的值</p>
<p>ctx-&gt;uid_set[0] = htonl(conf-&gt;service); // 与服务有关，为定值</p>
<p>ctx-&gt;uid_set[1] = htonl((uint32_t) ngx_time()); // 嗯，服务端的时间</p>
<p>ctx-&gt;uid_set[2] = htonl(start_value); // 不太清楚，从变量名来看应该为服务启动时间，启动后不会变化</p>
<p>ctx-&gt;uid_set[3] = htonl(sequencer_v2); // 一个sequence，每一次都会加，到了一定的阈值再重置</p>
<p>既然这样，那么上面说的第二个8位就是时间戳了。</p>
<p>由于该时间戳通过htonl进行了转换，按network byte order（也就是big endian）编码，我们使用时就需要调用ntohl转换成本地的字节顺序。</p>
<p>简单的c++程序如下：</p>
<blockquote><p>#include &lt;arpa/inet.h&gt;<br />
#include &lt;iostream&gt;<br />
using namespace std;<br />
int main(int argv, char ** argc){<br />
cout &lt;&lt; "ntohl" &lt;&lt; endl;<br />
cout &lt;&lt; ntohl(0xD616AD4D) &lt;&lt; endl;<br />
return 0;<br />
}</p></blockquote>
<p>输出的结果为 1303189206，嗯，后面的事情，大家都知道了。</p>
<p>如果用php，简单的代码如下：</p>
<blockquote><p>&lt;?php<br />
$str = "D616AD4D";<br />
$splited = str_split($str, 2);<br />
$splited = array_reverse($splited);<br />
$str = implode('', $splited);<br />
print hexdec($str);<br />
?&gt;</p></blockquote>
<p>其实就是自己手动实现了一把big endian 到 little endian的转换：将byte的次序换了一下。</p>
<p>以上纯属个人看法，如有不对，希望拍砖，谢谢~</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
