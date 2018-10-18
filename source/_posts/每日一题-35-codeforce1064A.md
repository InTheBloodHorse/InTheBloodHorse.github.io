---
title: 每日一题(35) codeforce1064A
date: 2018-10-18 19:01:14
tags:
- 数学
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1064/problem/A)
## 题意
给出三条边，问你能否组成三角形，如果可以就输出0，不能就输出边还要加上多少才可以组成三条边
## 思路
这是水题
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[5];
int main()
{
	for(int i=0;i<3;i++) cin >> a[i];
	sort(a,a+3);
	if(a[0]+a[1] > a[2] )cout << 0 << endl;
	else cout << a[2]-(a[0]+a[1])+1 << endl;
} 
```
