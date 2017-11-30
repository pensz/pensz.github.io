---
layout: post
status: publish
published: true
title: "关于bash的启动运行的文件"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 280
wordpress_url: http://www.zwsun.com/?p=280
date: '2012-12-01 21:19:07 +0000'
date_gmt: '2012-12-01 13:19:07 +0000'
categories:
- "技术记录"
tags:
- bash
---
<p>工作上经常碰到环境变量不对，bash运行行为和自己期望的不一致，于是想花点时间看看bash.</p>
<p>说起来，这件事情应该再简单不过了，看看bash手册上写的便是了.</p>
<p>http://www.gnu.org/software/bash/manual/bashref.html#Bash-Startup-Files</p>
<p>但是坦白来说，我花了很长时间才看明白了一点.</p>
<p>看到的一些结论<br />
如果是login的shell，那么bash在运行先执行 /etc/profile ；然后依次查找 ~/.bash_profile, ~/.bash_login, ~/.profile；如果找到其中一个文件就执行之，后面的就不执行了.</p>
<p>如果是interactive的非login shell，则先会执行 /etc/bash.bashrc，如果~/.bashrc存在的话，接着执行~/.bashrc.</p>
<p>如果是非interactive的，且非login shell，则执行 $BASH_ENV 对应的shell.</p>
<p>ssh到远端的机器，远端机器上的bash会执行什么呢？如果是login shell，不用说，和第1种一样；如果是ssh remote-machine 'cmd'这种的（也就是我们一般shell脚本里面调用形式），则会执行~/.bashrc（远端机器上的bash在编译bash的时候define了SSH_SOURCE_BASHRC ）.</p>
<p>有些问题<br />
什么是interactive shell，什么是login shell</p>
<p>interactive shell就是顾名思义，有用户交互的，检查方法就是在命令行下执行 echo $-，如果有i这个选项，则就是interactive shell 了.</p>
<p>login shell简单来说就是bash启动的时候加了 -l选项.</p>
<p>举几个实际的例子：</p>
<p>interactive & login shell:</p>
<p>linux登录到真实的终端tty1</p>
<p>interactive & non-login shell:</p>
<p>打开终端模拟器，在终端模拟器里面执行/bin/bash -i cmdfile</p>
<p>non-interactive & login shell</p>
<p>在终端模拟器里面执行/bin/bash -l cmdfile</p>
<p>non-interactive & non-login shell:</p>
<p>在终端模拟器里面执行/bin/bash cmdfile</p>
<p>为什么我在手动在机器上可以ssh到远端机器的，但是在shell脚本里面不行？</p>
<p>这种情形出现在ssh到远端机器的private key是有passphase 的，简单来说，ssh 到远端机器的时候，你还需要输入这个pass phase的，那为什么你一般ssh到远端机器的时候不要输入passphase呢，这个就是ssh-agent的功劳了，ssh通过环境变量中的SSH_AUTH_SOCK和SSH_AGENT_PID获得真实的private key。</p>
<p>简单来说，因为这个时候有了SSH相关环境变量。这些环境变量一般在通过keychain设置的，而keychain一般是startup脚本里面执行的。 解决办法就是bash -l.</p>
<p>骗人吧，按你的说法，ssh过去要执行~/.bashrc的，但是我~/.bashrc里面的命令没有执行</p>
<p>应该.bashrc里面有一段话，[ -z "$PS1" ] && return 也就是说，如果是非interactive的时候，PS1不会被设置，然后就return了，后面的命令当然没有执行.</p>
<p>interactive & login 的shell怎么也执行 ~/.bashrc 了？</p>
<p>看看 ~/.bash_profile, ~/.bash_login, ~/.profile 这几个文件中，应该有写</p>
<p>if [ -f "$HOME/.bashrc" ]; then<br />
    . "$HOME/.bashrc"<br />
fi<br />
ssh remote-machine 'cmd' 为什么不行啊？</p>
<p>一样的道理，PATH环境变量不是在 ~/.bashrc里面设置的.</p>
<p>ssh remote-mache 'cmd' 的时候不执行~/.bashrc啊？</p>
<p>编译bash的时候没有define SSH_SOURCE_BASHRC.</p>
<p>我的cmdfile在shebang部分里面已经声明了#!/bin/bash -l 了，但是它表现的不是login shell</p>
<p>不要使用 /bin/bash cmdfile 这种方式运行，要么使用cmdfile直接运行，要么使用/bin/bash -l cmdfile运行.</p>
