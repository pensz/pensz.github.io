---
layout: post
status: publish
published: true
title: "增加虚拟机空间的一些方法"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 232
wordpress_url: http://www.zwsun.com/?p=232
date: '2011-11-20 16:00:19 +0000'
date_gmt: '2011-11-20 08:00:19 +0000'
categories:
- "技术记录"
tags:
- virtualbox
---
<p><em>下面的讨论基于virtualbox(我选择virtualbox的原因是它免费，且支持各个平台)，虚拟机为linux，宿主操作系统不限。</em></p>
<p>经常一开始建了一个固定大小的虚拟硬盘用来安装虚拟机，但过段时间数据越来越多，最终发现其空间不够用，这时候就很郁闷了，但如果不是你系统的目录不够用的话，下面的办法可以解决空间不够的问题：</p>
<h2>1 增加一个虚拟硬盘的方法</h2>
<p>优点：虚拟机中读写速度较快，虚拟硬盘也可以后面给其他的虚拟机使用</p>
<p>缺点：宿主机器无法读写块虚拟硬盘中的内容</p>
<p>操作方法：</p>
<p>1.1 先给虚拟机配备一个动态大小的虚拟硬盘</p>
<p>这个和一般的装虚拟机新建虚拟硬盘一样的操作。</p>
<p>打开虚拟机的配置 &gt; storage 增加SATA控制器，然后在SATA控制器下增加一块虚拟硬盘；或者直接在IDE controller下面新增一块虚拟硬盘。</p>
<p><a href="http://www.zwsun.com/wp-content/uploads/2011/11/vmsetting.png"><img class="aligncenter size-full wp-image-237" title="vmsetting" src="http://www.zwsun.com/wp-content/uploads/2011/11/vmsetting.png" alt="" width="798" height="667" /></a></p>
<p>1.2 在虚拟机中进行分区，格式化，然后挂载</p>
<p style="padding-left: 30px;">1.2.1 分区：</p>
<p style="padding-left: 30px;">PS：必须要在root下操作，下面的描述中，<span style="color: #ff6600;">橙色字体</span>的为输入的命令。</p>
<p style="padding-left: 30px;">先用fdisk -l看看，如果没有找到你的虚拟硬盘，那就是前面的虚拟硬盘没有配置对。</p>
<p style="padding-left: 30px;">root@dev-desktop:/home/dev# <span style="color: #ff6600;">fdisk -l</span><br />
Disk /dev/sda: 8589 MB, 8589934592 bytes255 heads, 63 sectors/track, 1044 cylindersUnits = cylinders of 16065 * 512 = 8225280 bytesDisk identifier: 0x00015130<br />
Device Boot      Start         End      Blocks   Id  System/dev/sda1   *           1         993     7976241   83  Linux/dev/sda2             994        1044      409657+   5  Extended/dev/sda5             994        1044      409626   82  Linux swap / Solaris<br />
Disk /dev/sdb: 2845 MB, 2845835264 bytes255 heads, 63 sectors/track, 345 cylindersUnits = cylinders of 16065 * 512 = 8225280 bytesDisk identifier: 0x00000000<br />
<strong>Disk /dev/sdb doesn't contain a valid partition table</strong></p>
<p style="padding-left: 30px;">可以看到第二块虚拟硬盘 /dev/sdb 上面没有任何分区信息，当然无法用了。</p>
<p style="padding-left: 30px;">接着 fdisk /dev/sdb 在第二块虚拟硬盘上建立分区，关于fdisk的详细用法请在进入fdisk后按m或者搜索之。</p>
<p style="padding-left: 30px;">root@dev-desktop:/home/dev# <span style="color: #ff6600;">fdisk /dev/sdb</span><br />
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabelBuilding a new DOS disklabel with disk identifier 0xadff2d79.Changes will remain in memory only, until you decide to write them.After that, of course, the previous content won't be recoverable.<br />
Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)<br />
Command (m for help): <span style="color: #ff6600;">n</span></p>
<p style="padding-left: 30px;">Command action</p>
<p style="padding-left: 30px;">e   extended<br />
p   primary partition (1-4)<span style="color: #ff6600;">p</span></p>
<p style="padding-left: 30px;">Partition number (1-4): <span style="color: #ff6600;">1</span><br />
First cylinder (1-345, default 1): <span style="color: #ff6600;">1</span></p>
<p style="padding-left: 30px;">Last cylinder or +size or +sizeM or +sizeK (1-345, default 345):<br />
Using default value 345<br />
Command (m for help): <span style="color: #ff6600;">w</span></p>
<p style="padding-left: 30px;">The partition table has been altered!</p>
<p style="padding-left: 30px;">1.2.2 格式化</p>
<p style="padding-left: 30px;">有分区了，还是无法使用的，这时候需要格式化，linux下普遍使用的是ext3。我们通过mkfs对刚才的分区格式化：</p>
<p style="padding-left: 30px;">root@dev-desktop:/home/dev# <span style="color: #ff6600;">mkfs.ext3 /dev/sdb1 </span><br />
mke2fs 1.40.8 (13-Mar-2008)Filesystem label=OS type: LinuxBlock size=4096 (log=2)Fragment size=4096 (log=2)173888 inodes, 692795 blocks34639 blocks (5.00%) reserved for the super userFirst data block=0Maximum filesystem blocks=71303168022 block groups32768 blocks per group, 32768 fragments per group7904 inodes per groupSuperblock backups stored on blocks: 	32768, 98304, 163840, 229376, 294912<br />
Writing inode tables: done<br />
Creating journal (16384 blocks): doneWriting superblocks and filesystem accounting information: done<br />
This filesystem will be automatically checked every 38 mounts or180 days, whichever comes first.  Use tune2fs -c or -i to override.</p>
<p style="padding-left: 30px;">1.2.3 挂载</p>
<p style="padding-left: 30px;">通过mkdir先建立一个挂载点，如在根目录下建立一个data目录用于挂载 mkdir /data/</p>
<p style="padding-left: 30px;">root@dev-desktop:/home/dev# <span style="color: #ff6600;">mount /dev/sdb1 /data/</span></p>
<p style="padding-left: 30px;">通过df命令看看挂载情况，如果有相应的挂载信息就ok了。</p>
<p style="padding-left: 30px;">root@dev-desktop:/home/dev# <span style="color: #ff6600;">df -h</span><br />
Filesystem            Size  Used Avail Use% Mounted on<br />
/dev/sda1             7.6G  6.1G  1.2G  85% /<br />
varrun                352M  128K  352M   1% /var/run<br />
varlock               352M     0  352M   0% /var/lock<br />
udev                  352M   48K  352M   1% /dev<br />
devshm                352M     0  352M   0% /dev/shm<br />
lrm                   352M   40M  313M  12% /lib/modules/2.6.24-27-generic/volatile<br />
<strong>/dev/sdb1             2.7G   69M  2.5G   3% /data</strong></p>
<p style="padding-left: 30px;">1.2.4 设置 /etc/fstab</p>
<p style="padding-left: 30px;">做这一步的目的在于将第二块虚拟硬盘的信息写入系统分区信息表，便于开机自动挂载，否则，每次都需要手动去mount</p>
<p style="padding-left: 30px;">在目前的/etc/fstab文件后面增加一行</p>
<p style="padding-left: 30px;"><span style="color: #ff6600;">/dev/sdb1       /data           ext3   defaults 0 2</span></p>
<p style="padding-left: 30px;">关于fstab的配置和各个参数的含义，可以自行搜索之。</p>
<h2>2 和宿主机器通过数据空间共享的方法</h2>
<p>优点：宿主和虚拟机都可以操作共享的文件，虚拟机内的空间完全决定于宿主机器的物理硬盘和实际分区，虚拟机开启后也可以使用该办法。</p>
<p>缺点：速度较慢</p>
<p>方法：</p>
<p>在虚拟机的设置 &gt; 数据空间中设置宿主的目录和权限。</p>
<p>进入虚拟机后以root身份运行以下命令：</p>
<p><span style="color: #ff6600;">mount -t vboxsf  数据空间名称  挂载点</span></p>
<p><img class="aligncenter size-full wp-image-238" title="vm_share" src="http://www.zwsun.com/wp-content/uploads/2011/11/vm_share.png" alt="" width="798" height="667" /></p>
<p>总结：如果你在宿主能够访问虚拟机中文件这方面没有需求，推荐使用增加虚拟硬盘的方法。</p>
