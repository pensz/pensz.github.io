---
layout: post
status: publish
published: true
title: hive相关
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 264
wordpress_url: http://www.zwsun.com/?p=264
date: '2012-11-24 13:01:58 +0000'
date_gmt: '2012-11-24 05:01:58 +0000'
categories:
- "技术记录"
tags:
- hadoop
- hive
---
<p>最近一直使用hadoop hive做数据分析，遇到一些问题，也收获一些，记一下。</p>
<p><strong>一些经验和大家分享：</strong></p>
<p>1 使用external table会比较方便管理，将数据放到hdfs中，然后再基于hdfs建external表，这样你可以随心所欲的管理你的数据，可以使用hive的external table来查询，也可以自己直接写mapreduce job来处理；使用external table的另外一个好处是可以随便改变table的schema，因为在drop table时数据不会丢失；只是删除元信息，所以，如果schema需要调整，删掉重新建就是了。</p>
<p>2 能使用hql做的事情还是用hql吧，写mapreduce毕竟麻烦，数据调试是个问题。</p>
<p>3 hql和sql还是有些地方不一样的，比如group by的时候不能用column的alias，详情可以参考我的这个<a href="http://stackoverflow.com/questions/13470238/hiveql-udf-and-group-by/13489094#13489094">回答</a>。</p>
<p>4 hql里面的条件可以随便的使用like '%%' 了，因为基本上所有的hql都会转换成mapreduce job，所以，你使用like '%%'只是在map的时候稍微多了点计算量而已。</p>
<p>5 hwi玩玩还可以，但是无法真正用于生产环境：结果默认保存到内存中；输入的hql有错又不知道是什么错；跑一段时间会挂掉；界面极度难用等等，这些问题都是它被正式使用的障碍。</p>
<p><strong>一些问题的解决方法：</strong></p>
<p><strong>hive报metadata错：</strong></p>
<pre>
FAILED: Error in metadata: javax.jdo.JDOFatalInternalException: Unexpected exception caught.
NestedThrowables:
java.lang.reflect.InvocationTargetException
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask
</pre>
<p>解决方法：删掉 $HADOOP_HOME/build ，这种情况很有可能是大家的hadoop用的是src版本，然后编译出来有build，具体为什么会因此报错，还不得而知。</p>
<p><strong>hadoop datanode无法启动：</strong></p>
<p>这个主要是mac os上会遇到，在$HADOOP_HOME/logs/*datanode*out 中看日志，会发现有：</p>
<pre name="code">Unable to load realm info from SCDynamicStore</pre>
<p>在$HADOOP_HOME/conf/hadoop-env.sh 中设置一下HADOOP_OPTS即可</p>
<pre name="code" class="xml">export HADOOP_OPTS="-Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"</pre>
<p><strong>hwi无法启动：</strong></p>
<p><code>在$HIVE_HOME/conf/hdfs-site.xml中加入</code></p>
<pre name="code" class="xml">
&lt;property&gt;
    &lt;name&gt;hive.hwi.war.file&lt;/name&gt;
    &lt;value&gt;/lib/hive-hwi-0.8.1.war&lt;/value&gt;
    &lt;description&gt;注意上面的地，war换成你自己的包文件名称&lt;/description&gt;
&lt;/property&gt;
</pre>
