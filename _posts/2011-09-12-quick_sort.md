---
layout: post
status: publish
published: true
title: "快速排序"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 169
wordpress_url: http://www.zwsun.com/?p=169
date: '2011-09-12 23:55:52 +0000'
date_gmt: '2011-09-12 15:55:52 +0000'
categories:
- "技术记录"
- "数据结构和算法"
tags:
- c++
- "快速排序"
---
<p>快速排序是分治法的思想，先设定一个标志，保证左边的数据不大于该标志，右边的数据都不小于该标志。然后分别对左右区域使用同样的处理方法，最终得到的序列即为有序序列。</p>
<p>下面是 c++ 的快速排序。</p>
<pre name="code" class="c++">#include &lt;iostream&gt;
using namespace std;

/**
 * 调整数据，使得在一个位置左边的数据均小于 pivotkey， 右边的数据均大于 pivotkey
 */
int Partition(int *p, int low, int high) {
 int pivotkey = p[low];
 while (low &lt; high) {
 while (low &lt; high &amp;&amp; p[high] &gt; pivotkey) --high;
 p[low] = p[high];

 while (low &lt; high &amp;&amp; p[low] &lt;= pivotkey) ++low;
 p[high] = p[low];
 }

 p[low] = pivotkey;

 return low;
}

/**
 * 快速排序
 */
void QuickSort(int *p, int begin,  int end) {
 if (begin &lt; end) {
 int pivot;

 pivot = Partition(p, begin, end); // 进行一次分区
 QuickSort(p, begin, pivot-1); // 对左边的进行快速排序
 QuickSort(p, pivot+1, end); // 对右边进行快速排序
 }
}

int main() {
 int i, length, *p;
 cin &gt;&gt; length;
 p = (int *) malloc (length * sizeof(int));
 for (i=0; i&lt;length; ++i) {
 cin &gt;&gt; p[i];
 }

 cout &lt;&lt; "original" &lt;&lt; endl;
 for (i=0; i&lt;length; ++i) {
 cout &lt;&lt; p[i] &lt;&lt; endl;
 }    

 QuickSort(p, 0, length-1);

 cout &lt;&lt; "sorted" &lt;&lt; endl;
 for (i=0; i&lt;length; ++i) {
 cout &lt;&lt; p[i] &lt;&lt; endl;
 }

 return 0;
}</pre>
