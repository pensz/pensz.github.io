---
layout: post
status: publish
published: true
title: "关于php中的中文字符截取"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 141
wordpress_url: http://www.zwsun.com/?p=141
date: '2011-09-06 23:45:40 +0000'
date_gmt: '2011-09-06 15:45:40 +0000'
categories:
- "技术记录"
- "数据结构和算法"
tags:
- php
- "编码"
---
<p>php没有原生unicode支持，若没有mb（Multibyte String） 扩展，需要自己写截取中文字符串的代码。保证无乱码的关键在于保证截取的字节数正确，而这个参考编码规则即可。</p>
<p>如 utf8的编码规则：</p>
<pre>Char. number range | UTF-8 octet sequence
(hexadecimal) | (binary)
--------------------+---------------------------------------------
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
</pre>
<pre>110xxxxx 意味着 &gt;= 2^7 + 2^6 即 ASCLL值 &gt;= 192 时为两个字节
1110xxxx 意味着 &gt;= 2^7 + 2^6 + 2 ^ 5 即 ASCLL值 &gt;= 224 时为三个字节，以此类推。</pre>
<p>按上面的编码规则，可以先取一个字节，通过ASCLL值来判断应该截取字节的字节数。算法如下：</p>
<blockquote><p>初始化相关数据<br />
while(已经截取的长度 &lt; 要截取的长度) {<br />
临时字符 = 取当前位置后面的1个字节<br />
根据临时字符的ASCLL值判断本次截取的字节长度<br />
最终的字符串 += 从当前位置截取指定长度的字节<br />
当前位置 += 本次截取的字节长度<br />
已经截取的长度 += 本次截取的字节长度<br />
}</p>
<p>返回 最终的字符串</p></blockquote>
<p>于是有了下面这段经典的代码：</p>
<pre name="code" class="php">
function SubUtf8String($String,$Length) {
    $pos = 0;
    $lenCutted = 0;
    $stringCutted = "";
    $strlen = strlen($String);
    $Length = min($strlen, $Length);
    
    while ($lenCutted > $Length){
        $StringTMP = substr($String,$pos,1);
        
        $ascllvalue = ord($StringTMP);
        
        if ( $ascllvalue >= 224 ){
            $StringTMP = substr($String,$pos,3);
            $pos = $pos + 3;
            $lenCutted = $lenCutted + 3;
        } elseif ( $ascllvalue >= 192 ){
            $StringTMP = substr($String,$pos,2);
            $pos = $pos + 2;
            $lenCutted = $lenCutted + 2;
        } else {
            $pos = $pos + 1;
            $lenCutted = $lenCutted + 1;
        }
        $stringCutted .= $StringTMP;
    }
    
    if ($stringCutted){
        $stringCutted .= "...";
    }
    return $stringCutted;
}
</pre>
<p>gbk编码规则：<br />
00-7F 单字节情形<br />
81-FE 40-FE 双字节情形<br />
gb 18030的扩展部分<br />
81-FE 30-39 81-FE 30-39 四字节情形<br />
0x81308130到0xFE39FE39。四字节字符的第一个字节的编码为0x81至0xFE；第二个字节的编码范围为0x30至0x39；第三个字节编码范围为0x81至0xFE；第四个字节编码范围为0x30至0x39</p>
<p>根据上述规则，写出截取gbk编码的函数应该较为简单了。</p>
<p>&nbsp;</p>
<p>参考资料：</p>
<ul>
<li><a href="http://zh.wikipedia.org/wiki/GBK" target="_blank">http://zh.wikipedia.org/wiki/GBK</a></li>
<li><a href="http://tools.ietf.org/html/rfc3629" target="_blank">http://tools.ietf.org/html/rfc3629</a></li>
<li><a href="http://tools.ietf.org/html/rfc3629" target="_blank">http://www.php.net/manual/en/language.types.string.php</a></li>
</ul>
<p>&nbsp;</p>
