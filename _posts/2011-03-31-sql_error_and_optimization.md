---
layout: post
status: publish
published: true
title: sql常见错误和优化
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 20
wordpress_url: http://www.zwsun.com/?p=20
date: '2011-03-31 23:17:38 +0000'
date_gmt: '2011-03-31 15:17:38 +0000'
categories:
- "技术记录"
tags:
- mysql
- "优化"
---
<p>下面的讨论仅仅针对mysql，版本为5.0。</p>
<p>一、错误的sql</p>
<p>下面说的错误的sql都是语法没有错，但是实际的功能和原来的有很大的差异，在此一记以警戒自己。</p>
<p><strong>1 select 字段时忘记写逗号</strong></p>
<p>正确sql: select a,b from table</p>
<p>错误sql:select&nbsp; a b from table</p>
<p>要命的是这个sql并不报语法错误，本想取两个字段，结果是取了一个字段a，别名为b，有时候你查了半天为什么数据不对，结果是漏掉一个分号所致。</p>
<p><strong>2 delete只写了字段，没有写完整条件</strong></p>
<p>正确sql: delete from table where a = 1</p>
<p>错误sql:delete from table where a</p>
<p>上面的错误sql也不报语法错误，但是后果很严重，直接把所有的数据删掉。所以delete的时候一定要小心，同时建议delete时加上limit和更多确定的条件，这样增加一点点保险。</p>
<p><strong>3 update时条件不完整</strong></p>
<p>正确sql：update table table1, table2 set table1.a = '...' where table2.join = table1.join and table2.b = '**'</p>
<p>错误sql:&nbsp; update table table1, table2 set table1.a = '...' where table2.b = '**'</p>
<p>上面的错误sql杀伤力也是很大的，直接将table1的数据都修改一遍，我们同事就犯过这个错误，最后大家修数据修的快挂了。</p>
<p><strong>4 select join时，字段写错</strong></p>
<p>正确sql : select table1.a, table2.b from table1 inner join table2 on table1.join = table2.join</p>
<p>错误sql：select table1.a, table2.b from table1 inner join table2 on <strong>table1.join = table1.join</strong></p>
<p>注意后面的条件是同一个表的字段相等，这在任何情况下都是true，这样的话，内连接直接变成笛卡尔乘积，如果两个大表，系统直接挂掉，我犯过此错误，在测试环境上数据少，没仔细看结果，没有发现问题，上到正式环境上，mysql直接挂掉。</p>
<p><strong>5 时间没有加单引号</strong></p>
<p>正确sql：select a from table1 where d &gt; '2011-03-11'</p>
<p>错误sql: select a from table1 where d &gt; 2011-03-11</p>
<p>你会发现错误的sql后面的结果过滤没有达到你期望的结果。</p>
<p><br></p>
<p>二、mysql 优化部分</p>
<p>优化前建议先开启slowlog，分析mysql的slowlog，分析slowlog也有很多工具,自带的<a title="mysqldumpslow" href="http://dev.mysql.com/doc/refman/5.0/en/mysqldumpslow.html" _mce_href="http://dev.mysql.com/doc/refman/5.0/en/mysqldumpslow.html" target="_blank">mysqldumpslow</a>就还不错。</p>
<p>下面是自己总结的一些tips</p>
<p><strong>1 合理的表结构</strong></p>
<p>核心表中避免存大数据，允许合理的冗余数据存在。</p>
<p><strong>2 加索引</strong></p>
<p>这是最简单的办法，大部分情况下，是没有加索引，或者索引做的不对。是否该加索引，explain一下就知道了。</p>
<p><strong>3 避免join的字段类型不对</strong></p>
<p>同样的属性，在不同表里面保存的类型不一样，join完全无法使用索引，这时候如果无法对目前的表结构修改，可以使用cast作为暂时的解决方案。另外，join时，确保两个字段的确是同一个属性也非常重要，我们之前就犯过userId = user_id这样的低级错误，不但速度慢，而且结果也不对。</p>
<p><strong>4 根据目前的条件，推导出更加有效的条件</strong></p>
<p>如，根据实际需求有以下sql： select a.field1, b.field2 from a inner join b on a.join = b.join where {conditona}</p>
<p>如果你能够通过a表上的一条限制条件 {conditiona} 推导出一个b表上的限制条件{conditionb}，而 {conditionb} 又能够明显减少查询的记录或者能够有效的使用索引，那么加上{conditionb}后的效果是非常显著的。</p>
<p><strong>5 要多看看explain的结果</strong></p>
<p>即使同一个条件，条件里面的取值不一样，explain出来的结果也是不一样的。</p>
<p>当然，在数据量非常大时，explain的结果即使看起来很美好，但也可能很慢。</p>
<p><strong>6 业务上的，去掉返回大数据量的需求</strong></p>
<p>当一条sql要返回几十万或者更多的数据给用户看，这sql可能就不应该存在，和业务部门谈谈，也许他们根本不需要这样的结果。</p>
