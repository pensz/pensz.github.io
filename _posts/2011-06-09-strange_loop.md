---
layout: post
status: publish
published: true
title: php中"怪异"的循环
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 65
wordpress_url: http://www.zwsun.com/?p=65
date: '2011-06-09 22:32:49 +0000'
date_gmt: '2011-06-09 14:32:49 +0000'
categories:
- "技术记录"
tags:
- php
- foreach
---
<p>前不久在打酱油的时候发现了一个诡异的问题，遍历php数组，竟然结果有问题。具体就是最后一个元素的值和倒数第二个元素的值一样。最后打算去bugs.php.net上搜索一下是不是真的是bug，结果不是bug，有人也报告了这个问题，具体地址请移步至 http://bugs.php.net/bug.php?id=50582 。</p>
<p>问题代码：</p>
<blockquote>
<pre>Reproduce code:
---------------
$ii = array(1, 2, 3);
foreach ($ii as &amp;$i) echo $i;
foreach ($ii as $i) echo $i;

Expected result:
----------------
123123

Actual result:
--------------
123122</pre>
</blockquote>
<p>rasmus的解释如下：</p>
<blockquote>
<pre>It doesn't act weird.  There is no block scope in PHP, so at the end of 
the first loop $i is a reference to the last element of $ii and in the 
second loop you are now assigning values to that reference which means 
you are overwriting the 3rd element of $ii each time through the loop.  
That of course means that once you get to the 3rd element of $ii it is 
no longer 3 and you see the last value assigned to it, which was 2.</pre>
</blockquote>
<p>意思就是说：php中没有块作用域，故变量 $i 的作用域不仅仅存在于第一个循环，而是存在与整个代码中，经过第一个循环后，$i 为 $ii最后一个元素的引用；第二个循环的作用就是不断将当前数组中各个元素的值赋值给$ii的最后一个元素，故最后一个元素的值为倒数第二个元素的值。</p>
<p>解决方法就是:</p>
<p>1 第二个循环中不用 $i</p>
<p>2 第一个循环结束后 unset($i)</p>
