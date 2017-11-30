---
layout: post
status: publish
published: true
title: "经典算法问题-表达式求值"
author:
  display_name: pensz
  login: pensz
  email: email
  url: ''
author_login: pensz
author_email: email
wordpress_id: 199
wordpress_url: http://www.zwsun.com/?p=199
date: '2011-10-03 22:25:48 +0000'
date_gmt: '2011-10-03 14:25:48 +0000'
categories:
- "技术记录"
- "数据结构和算法"
tags:
- "表达式求值"
---
<p>上次和同事聊到了验证码中有表达式求值的问题，说起来应该是比较简单了，也是数据结构上经典的算法问题。可是自己写一写才会发现，做对一件看似简单的事情不是那么容易。</p>
<p>本来自己弄了一个c的stack，但是c没有模板，想不到怎么支持不同的类型，只好换用c++了。</p>
<pre name="code" class="c++">
#include &lt;stdio.h&gt;
#include &lt;ctype.h&gt;
#include &lt;stdlib.h&gt;

#include &lt;stack&gt;
#include &lt;iostream&gt;

using namespace std;


/**
 * 比较运算符之间的优先级
 */
static short int Precede(char c1, char c2) {
    if (c1 == c2) {
        if (c1 == '#') {
            return 0;
        } else if(c1 == '(') {
            return -1;
        } else {
            return 1;
        }
    }
    
    switch (c1) {
        case '+':
        case '-':
        
        if (c2 == '*' || c2 == '/' || c2 == '(') {
            return -1;
        } else {
            return 1;
        }
        break;
        
        case '*':
        case '/':
        if (c2 == '(') {
            return -1;
        } else {
            return 1;
        }
        break;
        
        case '(':
        if (c2 == ')') {
            return 0;
        } else {
            return -1;
        }
        break;
        
        case ')':
        return 1;
        break;
        
        case '#':
        return -1;
    }
}

/**
 * 对加减乘除进行求值
 */
static float Operate(float a, char theta, float b) {
    switch (theta) {
        case '+':
        return a+b;
        break;
        
        case '-':
        return a-b;
        break;
        
        case '*':
        return a*b;
        break;
        
        case '/':
        return a/b;
        break;
    }
    
    printf("bad operator");
    return 0;
}


int main() {
    
    stack&lt;char&gt; optr;
    stack&lt;float&gt; opnd;
    
    optr.push('#');
    
    char theta, tmp[20], *tmppoint, basechar[20];
    tmppoint = basechar;
    float a, b;
    
    char c = getchar();
    
    while (c!='#' || optr.top() != '#') {
        if (isdigit(c)) {
            *(tmppoint++) = c;
            c = getchar();
        } else {
            
            if (tmppoint > basechar) {
                *tmppoint = '<pre wp-pre-tag-0></pre>';
                opnd.push(atof(basechar));
                printf ("push number %s\n", basechar);
                tmppoint = basechar;
            }
            
            if (c != '+' && c != '-' && c != '*' && c != '/' && c != '(' && c != ')' && c != '#' ) {
                c = getchar();
                continue;
            }
            
            short int precede = Precede(optr.top(), c);
            switch (precede) {
                case -1:
                optr.push(c);
                printf("pushed operator %c \n", c);
                c = getchar();
                break;
                
                case 0:
                optr.pop();
                c = getchar();
                break;
                
                case 1:
                theta = optr.top();
                optr.pop() ;
                
                if (opnd.empty()) {
                    printf("opnd is empty\n");
                }
                b = opnd.top();
                opnd.pop();
                
                a = opnd.top();
                opnd.pop();
                
                cout << " cal a=" << (float)a << " theta=" << theta << " b=" << (float)b << endl;

                opnd.push(Operate(a, theta, b));
                break;
            }
        }
    }
    
    /* 最后的结果 */
    printf("%f\n", opnd.top());
    printf("%c\n", optr.top());
    
    return 0;
}
</pre>
