---
layout: post
status: publish
published: true
title: "悲惨的mac输入法更新经历"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 114
wordpress_url: http://www.zwsun.com/?p=114
date: '2011-08-07 10:43:18 +0000'
date_gmt: '2011-08-07 02:43:18 +0000'
categories:
- mac os
tags:
- mac
- "输入法"
---
<p>江湖盛传，最近mac输入法风生水起，各家已经做的非常nb了。</p>
<p>我本不想再折腾这些软件了，实在没有意思；再说了，当年google出的mac输入法beta版害死一大堆人的情景历历在目（注销后登录无法输入密码），但考虑目前使用的sunpinyin和FIT在词汇量方面太少，经常影响打字的速度，故想试试新的输入法；怎么说也有两大公司推出了mac下的输入法，也都推出了正式版，应该比较稳定可靠吧。</p>
<p>一开始就卸载了FIT，因为我用它不是很爽。再比较一下两大输入法公司的产品：qq mac输入法和搜狗mac输入法</p>
<p>通过网站对比发现，qq的应该更加好点，说明详细，更新频繁，应该有保证；而搜狗的mac输入法更新不是很及时，另外说明文档没有qq那边多。这么想想，应该qq的输入法比较有保证，下载下来安装。</p>
<p>安装一切顺利，只是最后安装uninstaller时失败，我想想不是什么大事，就在自定义安装时，去掉了uninstaller。安装后，切换到qq拼音，系统巨卡无比，我想，初始化可能会慢点，结果后面还是巨卡，输入法设置没有出现，无法正常输入。打开活动监视器看，发现ReportCrash飘过，也就是说程序出现异常了。难道需要重启？重启了看看，结果现象依旧。我无语了，准备卸掉，结果卸载程序无法运行（应该是只能10.6以上的版本运行，从程序的异常log中也发现了一句 unknown required load command 0x80000022 ），最后只好通过命令行卸载了。</p>
<p>那接着只能看看搜狗的怎么样了，下载下来，兴冲冲的安装，结果提示我该输入法只支持10.6以上的系统。我还能说什么呢？唯有放弃。</p>
<p>虽然是两家大公司，但好像都没有打算支持10.5 leopard系统的意思，在网站上面也没有看到说明仅支持10.6以上的系统，不得不说这是个很大的遗憾。</p>
<p>对了，还有<a title="sunpinyin official site" href="http://sunpinyin.org" target="_blank">sunpinyin</a>可以更新看看，抱着一丝希望，更新至2.0.3版本，结果还是挂了，现象和qq拼音一样。</p>
<p><a href="http://www.zwsun.com/wp-content/uploads/2011/08/sunpinyin.png"><img class="aligncenter size-full wp-image-119" title="sunpinyin" src="http://www.zwsun.com/wp-content/uploads/2011/08/sunpinyin.png" alt="sunpinyin crash" width="198" height="241" /></a></p>
<p>&nbsp;</p>
<p>正如你上面看到的那样，配置面板一直无法载入；而活动监视器里面也有ReportCrash的进程。虽然网站上写的支持10.5/6，但我的确无法使用，有可能是我安装不对，也有可能没在10.5(leopard) 上面好好测试过。</p>
<p>再找找QIM看看了，QIM之前是mac下最好的输入法，最近听说免费了。找了很久，有新闻，有链接，但无法下载，我很无奈和绝望。</p>
<p>难道只能安装FIT了吗？对于这个输入法，我今天一开始就卸了它，为什么？因为我对它很有成见，之前不好好做核心功能，反而在状态栏上面多增加一个图标，被大家狠狠一顿批；还做什么截屏功能，要知道，截屏是mac自带的功能（最简单的是按 shift+cmd+4）；另外给我的感觉是反应较慢，耗内存较多。</p>
<p>但现在是没有选择了，只好硬着头皮去下载FIT了，最后通过下载FIT，奇迹般的找到了一个网站，可以下载QIM。这个网站（http://mac.brothersoft.com/）很赞，有很多mac下的软件，而且可以直接wget下（为什么直接wget下，你懂的），如果你是10.5的系统，可以用这个链接下载（<a title="download QIM" href="http://macfiles.brothersoft.com/business/word_processing/QIM_Sogou_2_0_0_Leopard.dmg.zip" target="_blank">http://macfiles.brothersoft.com/business/word_processing/QIM_Sogou_2_0_0_Leopard.dmg.zip</a>）。</p>
<p>最后，终于用上了QIM，QIM的确不错，安装后不需要设置就能很方便的使用了，词汇量大，反应速度也很快；这篇blog就是用QIM写的。</p>
<p><strong>结论是：如果你是mac os 10.5的系统，我建议你使用<a href="http://macfiles.brothersoft.com/business/word_processing/QIM_Sogou_2_0_0_Leopard.dmg.zip" target="_blank">QIM</a>，其次是<a href="http://funinput.com/mac" target="_blank">FIT</a>2.2.1（2.3.0后无法使用，情况和sunpinyin一样，无法载入配置面板），再者是<a href="http://code.google.com/p/sunpinyin/downloads/list" target="_blank">sunpinyin</a> 2.0.2 （更新到2.0.3后无法使用了，不知道是个人特例还是普遍情况）。</strong></p>
<p>非常感谢这么多个人开发了这么好的免费的软件供大家使用。</p>
