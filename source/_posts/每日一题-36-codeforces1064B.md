---
title: 每日一题(36) codeforces1064B
date: 2018-10-19 14:55:55
tags:
- 二进制
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1064/problem/B)
## 题意
输出 2的 (数字n的二进制里面的1的个数的) 次方
## 思路
发现了一个超级厉害的函数。_builtin_popcount()计算二进制中多少个1。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int n;
	int t;
	cin >> n ;
	for(int i=0;i<n;i++){
		cin >> t;
		cout << (1<< __builtin_popcount(t)) << endl;
	}	
} 
```


