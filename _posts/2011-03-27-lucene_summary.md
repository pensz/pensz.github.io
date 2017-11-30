---
layout: post
status: publish
published: true
title: lucene 相关小结
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 17
wordpress_url: http://www.zwsun.com/?p=17
date: '2011-03-27 22:56:16 +0000'
date_gmt: '2011-03-27 14:56:16 +0000'
categories:
- "技术记录"
tags:
- lucene
---
<p>前不久工作中重新使用了一下lucene，比以前更加清晰点了，记点小结，以备后用。</p>
<p>1 Document 类：</p>
<p>此类是索引文档的最小单元，它可以理解成一个Hashmap，可以有各种各样的属性(Field)，任何你要索引的内容都需要转成这个Document才能加入到索引中。</p>
<p>2 Analyzer 类：</p>
<p>分词器，建索引时，将文章中的text分成一个一个的term，然后term做反向索引。搜索时，将搜索的词分成一个一个的term，然后到索引中查找，目前lucene中自带的smartcn分词器用来中文分词也还不错。</p>
<p>3 几个疑问：</p>
<p>Q： lucene的IndexWriter 和 IndexReader是线程安全的吗？</p>
<p>A：答案是线程安全，IndexReader和IndexWriter的关系<strong>大概</strong>就是读写锁的关系。</p>
<p>Q：可以对一个目录同时进行索引和搜索的过程吗？</p>
<p>A：当然是了，这里我之前的疑问是：如果我建索引的过程中，删了某些Document，搜索是不是马上就搜不出结果了？答案是这种情况不会出现，搜索还是可以搜出来的，需要IndexWriter.close()且对IndexReader进行reopen才能起效。reopen的使用可以参考<a title="reopen" href="http://lucene.apache.org/java/3_0_3/api/core/org/apache/lucene/index/IndexReader.html#reopen%28%29" target="_blank">apache的文档</a>。</p>
<p>Q：是否可以按某个条件搜索索引，然后删除这部分Document？</p>
<p>A：可以，参考<a href="http://lucene.apache.org/java/3_0_3/api/core/org/apache/lucene/index/IndexWriter.html#deleteDocuments%28org.apache.lucene.search.Query...%29" target="_blank">官方文档</a>，而且还有updateDocument可用。</p>
