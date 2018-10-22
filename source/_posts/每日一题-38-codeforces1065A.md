---
title: 每日一题(38) codeforces1065A
date: 2018-10-22 20:53:19
tags:
- 数学
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1065/problem/A)
## 题意
s，a，b，c代表有s卢布，每买a个，就送b个，单价为c。输出最多可以买到多少个商品。
## 题意
一个公式就好了。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	 int t;
	 long long a,b,c,r;
	 cin >> t;
	 for(int i=0;i<t;i++){
	 	cin >> r >> a >> b >> c;
	 	cout << r/c + (r/c)/a*b << endl;
	 }
} 
```

