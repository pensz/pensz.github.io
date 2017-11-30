---
layout: post
status: publish
published: true
title: "更新recovery的一个巧妙方法"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 33
wordpress_url: http://www.zwsun.com/?p=33
date: '2011-04-12 22:27:02 +0000'
date_gmt: '2011-04-12 14:27:02 +0000'
categories:
- "技术记录"
tags:
- android
- recovery
- "刷机"
---
<p><em>这是我当时刷新我手机recovery所使用的一种方法，在此备份一下，如果你的recovery无法刷新，可以试试用这个方法。</em></p>
<p>看了几篇更新recovery1.5.2的帖子，总体感觉是非常简单的，核心就是一条命令。<br />
但tim版（32A SPL 1.33.0008 S-ON，目前装rom为安卓3.04）却更新失败，这些问题经过搜索也没有满意的结果。<br />
经过自己的实验，发现一种可行的办法，和大家分享一下，希望能够有用，如有不对，还请诸位拍砖。</p>
<p>常用的刷recovery1.5.2 方法可能会有以下问题：</p>
<p>1 <strong>out of memory </strong>错误<br />
使用的命令：<br />
flash_image recovery recovery.img<br />
问题的具体情况可以参考这个链接： <a href="http://www.hiapk.com/bbs/thread-63257-1-1.html" target="_blank">http://www.hiapk.com/bbs/thread-63257-1-1.html</a></p>
<p>2 <strong>FAILED (remote: signature verify fail)</strong> 错误<br />
使用的命令：<br />
fastboot flash recovery img/recovery.img<br />
既然flash_img不行，那就使用fastboot，结果发现报签名错误，应该是s-on的缘故吧。<br />
（xda上推荐的方法就是刷spl了，可是现在我的recovery都进不去了，而且我还不想刷spl）</p>
<p>2 <strong>header is the same, not flashing recovery 错误</strong><br />
该方法已经有点麻烦了，原理是先使用recovery.img 启动，然后再刷recovery。</p>
<p>引用帖子（<a href="http://www.android.net/bbs/viewthread.php?tid=9926&amp;" target="_blank">http://www.android.net/bbs/viewthread.php?tid=9926&amp;</a>;extra=pageD1&amp;page=1）：</p>
<div>
<blockquote><p>1. 进入fastboot模式<br />
2. 跟电脑连上，装驱动什么的当然要<br />
3. fastboot boot recovery.1.4.img<br />
boot后面跟要刷的recovery的镜像<br />
adb shell mount -a<br />
挂载一切设备（指手机内的ROM和SD卡），忽略其他错误<br />
adb push recovery.1.5.img /system/recovery.img<br />
adb push recovery.1.5.img /sdcard/recovery.1.5.img<br />
adb shell flash_image recovery /sdcard/recovery.1.5.img<br />
4. 重启</p></blockquote>
</div>
<p>结果还是不行，报上面的错。</p>
<div>
<blockquote><p>header is the same, not flashing recovery</p></blockquote>
</div>
<p>大概意思是文件的内容一样，既然这样，本人就想了一个投机的方法：<br />
将启动所使用的recovery改成一个低版本的，然后刷1.5.2的recovery<br />
具体方法如下：</p>
<div>
<blockquote><p>下载一个老一点的recovery版,我下的是1.3.2，地址：<a href="http://forum.xda-developers.com/showthread.php?t=530492" target="_blank">http://forum.xda-developers.com/showthread.php?t=530492</a><br />
1. 进入fastboot模式（手机开机时按返回键和电源键）<br />
2. 跟电脑连上，装好驱动，开启usb调试模式<br />
3. 在sdk目录的命令行下输入 fastboot boot recovery.1.3.2.img<br />
（此处的 recovery.1.3.2.img 即为刚下的老版本recovery，无需拷贝至手机）<br />
手机再次重启，进入1.3.2的recovery模式<br />
然后在pc cmd端上输入以下命令：<br />
adb shell mount -a<br />
（挂载一切设备（指手机内的ROM和SD卡），忽略其他错误）<br />
adb push recovery.1.5.2.img /system/recovery.img<br />
adb push recovery.1.5.2.img /sdcard/recovery.1.5.2.img<br />
adb shell flash_image recovery /sdcard/recovery.1.5.2.img<br />
4. 重启</p></blockquote>
</div>
<p><span style="color: red;"><strong>最后提醒大家，刷机有风险，刷前需谨慎。</strong></span></p>
