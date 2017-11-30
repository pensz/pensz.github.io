---
layout: post
status: publish
published: true
title: hadoop trails
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 265
wordpress_url: http://www.zwsun.com/?p=265
date: '2012-11-24 12:17:42 +0000'
date_gmt: '2012-11-24 04:17:42 +0000'
categories:
- "技术记录"
tags:
- hadoop
---
<p>最近在捣鼓hadoop相关东东，部门里面需要总结一个hadoop相关的trails，方便后面freshmen加入时可以参考。写了写，可能不对，供新手参考。自己也还是一个新手啊。</p>
<p>hadoop还是很多值得学习的地方，hadoop建立者Dogg Cutting 也是lucene的作者，现在又是ASF的主席。</p>
<p>&nbsp;</p>
<div>
<h1>名词解释</h1>
<p>与hadoop关联的软件、名词非常多，这里列出一些常用的。</p>
<ul>
<li>hadoop: 一整套分布式存储、计算解决方案</li>
<li>hdfs: 一套分布式文件系统</li>
<li>mapreduce: 一套分布式计算框架</li>
<li>hbase: 基于hdfs的column family 的nosql数据库</li>
<li>hive: 一套将类似sql的hql转换成mapreduce job的工具</li>
<li>thrift: 一套用于各种语言间通讯的协议以及实现</li>
</ul>
<h2>搭建环境</h2>
<h3>搭建基本的hadoop环境</h3>
<ul>
<li>在本地环境搭建伪分布式的hadoop环境，包括基本的hdfs和mapreduce</li>
<li>在本地环境搭建hive和hbase等关联的环境</li>
</ul>
<h3>检验</h3>
<ul>
<li>本地能够搭建一个hadoop环境，并且实现以下功能</li>
<li>将本地文件放入hdfs</li>
<li>运行一个基于hdfs的mapreduce示例程序</li>
</ul>
<h2>使用hdfs</h2>
<h3>需要知道的知识</h3>
<ul>
<li>hadoop文件存放的方式</li>
<li>hadoop对文件系统操作命令行的使用</li>
</ul>
<h3>检验</h3>
<ul>
<li>上传文件到hdfs</li>
<li>从hdfs中获取文件到本地</li>
</ul>
<h2>使用mapreduce</h2>
<h3>需要知道的知识</h3>
<ul>
<li>基本的java面向类编程</li>
<li>mapreduce的过程</li>
<li>如何自己写一个mapreduce程序</li>
<li>如何运行mapreduce程序</li>
</ul>
<h3>检验</h3>
<ul>
<li>知道什么逻辑应该在map端处理，什么逻辑在reduce时处理</li>
<li>写一个程序统计一个文件中各个单词出现的次数</li>
</ul>
<h2>进一步了解hdfs</h2>
<h3>需要知道的知识</h3>
<ul>
<li>hdfs的架构</li>
<li>namenode和datanode的作用</li>
<li>hdfs中的block</li>
</ul>
<h3>检验</h3>
<ul>
<li>描述一个客户端上传文件到hdfs时，各个结点之间的通讯次序。</li>
</ul>
<h2>进一步了解mapreduce</h2>
<h3>需要知道的知识</h3>
<ul>
<li>数据如何在map和reduce过程传递的</li>
<li>job是如何分配的</li>
<li>输入的数据是如何分配给map程序的</li>
<li>map计算出来的结果是如何组织的，然后传递给reduce的</li>
<li>一个任务map的个数取决于哪些因素</li>
</ul>
<h3>检验</h3>
<ul>
<li>描述一个客户端提交了一个mapreduce的任务，hadoop内部各个结点做了哪些动作。</li>
<li>自定义mapreduce的InputFormat, OutputFormat, Writable</li>
</ul>
</div>
<p>&nbsp;</p>
