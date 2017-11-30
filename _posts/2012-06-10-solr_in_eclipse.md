---
layout: post
status: publish
published: true
title: "在eclipse中调试solr"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 252
wordpress_url: http://www.zwsun.com/?p=252
date: '2012-06-10 14:43:50 +0000'
date_gmt: '2012-06-10 06:43:50 +0000'
categories:
- "自言自语"
tags: []
---
<p>最近需要查看搜索执行代码的情况，故需要在eclipse中调试solr，网上出名那篇来自lucid的<a title="setting up sold in eclipse" href="http://www.lucidimagination.com/devzone/technical-articles/setting-apache-solr-eclipse" target="_blank">《setting up apache solr in eclipse》</a>，但个人觉得不是很方便。</p>
<p>自己看了一下，可以使用以下方法：</p>
<p>1 下载solr的src包，并解压</p>
<p>2 解压后，在解压后的根目录执行ant eclipse，即生成eclipse需要的项目文件</p>
<p>打开eclipse，File &gt; Import &gt; Existing Projects into Workspace</p>
<p>选择刚才解压后的根目录，这时候java build path等都已经设置好了。</p>
<p>3 Open Type找到StartSolrJetty 这个类，修改main方法里面的setPort参数为默认的8983，以及ContextPath,War</p>
<p>War为"solr/webapp/web/"</p>
<p>最后的代码应该是这样的：</p>
<blockquote><p>Server server = new Server();</p>
<p>SocketConnector connector = new SocketConnector();</p>
<p>// Set some timeout options to make debugging easier.</p>
<p>connector.setMaxIdleTime(1000 * 60 * 60);</p>
<p>connector.setSoLingerTime(-1);</p>
<p>connector.setPort(8983);</p>
<p>server.setConnectors(new Connector[] { connector });</p>
<p>WebAppContext bb = new WebAppContext();</p>
<p>bb.setServer(server);</p>
<p>bb.setContextPath("/solr");</p>
<p>bb.setWar("solr/webapp/web");</p></blockquote>
<p>&nbsp;</p>
<p>4 设置solr.solr.home，并run</p>
<p>在run configure中Arguments &gt; VM arguments中写入</p>
<p>-Dsolr.solr.home=solr/example/solr</p>
<p>使用solr自带的一个example作为sold配置的根目录，如果你有其他的solr配置目录，设置之即可。</p>
<p>点击run即可，debug也是一样可以用了。</p>
