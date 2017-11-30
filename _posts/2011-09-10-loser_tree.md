---
layout: post
status: publish
published: true
title: "败者树"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 150
wordpress_url: http://www.zwsun.com/?p=150
date: '2011-09-10 23:49:35 +0000'
date_gmt: '2011-09-10 15:49:35 +0000'
categories:
- "技术记录"
- "数据结构和算法"
tags:
- php
- "败者树"
---
<p>很多时候看起来很简单，但是最好自己写一写，这样理解才会更加深刻一些。下面是今天练手写的败者树php实现。</p>
<pre name="code" class="php">


// 测试数据
$data_input = array(
    array(2, 5, 10, 14, 20, 21, 23, 25, 30),
    array(1, 6, 9,  13, 17),
    array(3, 8, 12, 16, 19),
    array(4, 7, 11, 15, 18),
);

// 调用代码示例
k_merge($data_input); 
find_x($data_input, 10);


global $b, $k; // $b 为败者树上的叶子结点值， $k为归并的路数量
define('MINVALUE', -65535); // 定义最小值
define('MAXVALUE', 65536); // 定义最大值

/**
 * k 路归并排序
 */
function k_merge($data_input) {
    global $b, $k;
    $k = count($data_input);

    for ($i = 0; $i< $k; $i++) {
        $b[$i] = array_shift($data_input[$i]);
    }

    $loser_tree = array();
    create_loser_tree($loser_tree);

    while ($b[$loser_tree[0]] !== MAXVALUE){
        $key = $loser_tree[0];
        $value = $b[$key];
        print "\n " . $value . " from $key" ;
        $b[$key] = array_shift($data_input[$key]);
        $b[$key] = $b[$key] === null ? MAXVALUE : $b[$key];
        adjust_loser_tree($loser_tree, $key);
    }
}

/**
 * 从 k 段有序序列中找到 第 x 小的数据
 */
function find_x($data_input, $x) {
    global $b, $k;
    $k = count($data_input);

    for ($i = 0; $i< $k; $i++) {
        $b[$i] = array_shift($data_input[$i]);
    }

    $loser_tree = array();
    create_loser_tree($loser_tree);

    for ($x_i = 1; $x_i <= $x - 1; $x_i++) {
        $key = $loser_tree[0];
        $value = $b[$key];
        // print "\n -- $key " . $value ;
        $b[$key] = array_shift($data_input[$key]);
        $b[$key] = $b[$key] === null ? MAXVALUE : $b[$key];
        adjust_loser_tree($loser_tree, $key);
    }

    $key = $loser_tree[0];
    $value = $b[$key];
    print "\n" . $value . "\n";
}

/**
 * 创建败者树
 */
function create_loser_tree(&$loser_tree) {
    global $b, $k;
    $b[$k] = MINVALUE; //下面初始化调整败者树会用到

    for ($i = 0; $i<$k; $i++) {
        $loser_tree[$i] = $k;
    }

    for ($i = $k - 1; $i >=0; $i--) {
        adjust_loser_tree($loser_tree, $i);
        // var_dump(" adjusted $i", $loser_tree);
    }
}

/**
 * 重新调整败者树
 */
function adjust_loser_tree(&$loser_tree, $s) {
    global $b, $k;
    $t = intval(($s + $k) / 2);

    while ($t > 0) {
        if ($b[$s] > $b[$loser_tree[$t]]) {
            $temp = $s;
            $s = $loser_tree[$t];
            $loser_tree[$t] = $temp;
        }
        $t = intval($t / 2);
    }
    
    $loser_tree[0] = $s;
}

</pre>
