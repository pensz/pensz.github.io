---
layout: post
status: publish
published: true
title: python中的module
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 50
wordpress_url: http://www.zwsun.com/?p=50
date: '2011-04-25 22:28:40 +0000'
date_gmt: '2011-04-25 14:28:40 +0000'
categories:
- "技术记录"
tags:
- python
- module
- package
---
<p>很惭愧，虽然之前用python打过几次酱油，也号称自己看了那个简明教程，但事实上，我还是不熟呐。</p>
<p>恰巧最近又被python的timezone处理搞的极其郁闷，那么记一点自己看到的关于package和module的资料吧，日后好查。</p>
<p>说白了，package是一个文件夹，文件夹下面有__init__.py，而module对应的是文件，文件名为module.py。</p>
<p>import 和 from import 有以下几种格式：</p>
<p>import module #导入module，module在python可以搜索到的path中</p>
<p>from package import module # 导入package中的module，package在python可以搜索到的path中</p>
<p>from module import function # 导入module中的function</p>
<p>from package import * # 导入package中所有的module，哪些module需要导入？由__init__.py 中的__all__决定</p>
<p>from module import * # 导入module中所有的变量</p>
<p>可以看到from import的用法还是很复杂的。</p>
<p>另外一个简单的规则，import后面的变量名可以直接用了。</p>
<p>&nbsp;</p>
<p>参考资料：</p>
<p>http://docs.python.org/tutorial/modules.html</p>
<p>http://docs.python.org/reference/simple_stmts.html#the-import-statement</p>
<p>http://www.effbot.org/zone/import-confusion.htm</p>
<p>http://woodpecker.org.cn/diveintopython/object_oriented_framework/importing_modules.html</p>
