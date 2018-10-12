---
title: 每日一题(25) codeforces 1051B
date: 2018-10-08 21:06:46
tags:
- 数论
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1051/problem/B)
## 题意
给两个数字 n，m，将[n，m]区间拆成 长度/2 个 pair，每个pair包含两个数字，要求两个数字互质，如果不能拆成 长度/2个，则输出NO
## 思路
一开始还以为是找素数。想复杂了，因为任意相邻的两个数，都是互质的！
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	long long x,y;	
	cin >> x >> y;
	cout << "YES" << endl;
	for(long long i = x;i<y;i+=2){
		cout << i << " " << i+1 << endl;
	}
} 
```
