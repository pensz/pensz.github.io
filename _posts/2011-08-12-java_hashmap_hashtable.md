---
layout: post
status: publish
published: true
title: java中HashMap和HashTable的比较
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 124
wordpress_url: http://www.zwsun.com/?p=124
date: '2011-08-12 20:41:38 +0000'
date_gmt: '2011-08-12 12:41:38 +0000'
categories:
- "技术记录"
tags:
- java
- hash
---
<p>1 HashMap 和 HashTable 都是通过链接法解决碰撞</p>
<p style="text-align: center;"><a href="http://www.zwsun.com/wp-content/uploads/2011/08/hashtable.png" target="_blank"><img class="aligncenter size-medium wp-image-125" title="hashtable" src="http://www.zwsun.com/wp-content/uploads/2011/08/hashtable-300x164.png" alt="通过链接法解决碰撞，点击放大图片" width="300" height="164" /></a></p>
<p style="text-align: center;">链接解决碰撞，摘自算法导论第二版135页</p>
<p><a href="http://www.zwsun.com/wp-content/uploads/2011/08/hashtable.png"></a><br />
2 HashMap 的hash函数更加牛一些，另外支持null的key（这其实是一个支不支持的问题）</p>
<p>3 javadoc中写道：除了不同步和允许使用 null 之外，HashMap 类与 Hashtable 大致相同。从具体代码来看，HashMap代码的方法前面没有加synchronized，而HashTable的方法前面加了synchronized；另外，HashTable不支持null是“全面”的，key 和 value都不能为null</p>
<p>4 HashMap计算key对应的bucket（桶）所使用取模算法中，HashMap是二进制的 &amp; (table.length - 1)，而HashTable 是 % table.length，显然HashMap更加牛些</p>
<p>5 HashMap的查找，插入性能更加好，因为它中间会对key的hashCode再做一次hash，使其尽量均匀分布，而HashTable基本依赖于key的hashCode()</p>
<p>选择的原则：尽量使用HashMap，如果有同步方面的需求，自己可以在外面做控制。</p>
