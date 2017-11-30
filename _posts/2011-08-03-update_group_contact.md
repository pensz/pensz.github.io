---
layout: post
status: publish
published: true
title: update使用group_contact
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 107
wordpress_url: http://www.zwsun.com/?p=107
date: '2011-08-03 22:09:16 +0000'
date_gmt: '2011-08-03 14:09:16 +0000'
categories:
- "技术记录"
tags:
- sql
- update
- group
---
<p>以下讨论针对mysql。</p>
<p>想要达到的目的：</p>
<p>UPDATE users AS u<br />
LEFT JOIN orders AS o ON o.user_id = u.user_id<br />
SET u.orders = GROUP_CONCAT(DISTINCT o.order_id)</p>
<p>上面的sql无法运行，即使加上GROUP BY也是不行的</p>
<p>UPDATE users AS u<br />
LEFT JOIN orders AS o ON o.user_id = u.user_id<br />
SET u.orders = GROUP_CONCAT(DISTINCT o.order_id)<br />
GROUP BY u.user_id</p>
<p>一种的不用临时表的解决方法为：<br />
UPDATE users AS u<br />
SET u.orders = (<br />
SELECT GROUP_CONCAT(DISTINCT o.order_id)<br />
FROM orders o<br />
WHERE o.user_id = u.user_id<br />
)</p>
