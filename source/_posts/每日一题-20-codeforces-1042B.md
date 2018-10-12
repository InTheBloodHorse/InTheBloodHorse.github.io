---
title: 每日一题(20) codeforces 1042B
date: 2018-10-01 18:54:06
tags:
- STL
- Map
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1042/problem/B)
## 题意
给一个数字n
输入 a，b 代表用 a元可以买到 b果汁，b果汁有A,B,C三种维生素，比如AB 代表有A,B两种维生素。
假如可以补充ABC这三种维生素，输出最少花费的金额。
否则输出-1
## 思路
就几种可能性，暴力枚举就好了
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
long long INF = 1e9;
map<string,long long>m;
int main()
{
	int n;
	long long p;
	string a;
	m["A"] = INF;        
	m["B"] = INF;        
	m["C"] = INF;        
	m["AB"] = INF;        
	m["AC"] = INF;        
	m["BC"] = INF;        
	m["ABC"] = INF;
	cin >> n;
	for(int i=0;i<n;i++)
	{
		cin >> p >> a;
		sort(a.begin(),a.end());
		m[a] = min(m[a],p);	 
	}
	long long ans = INF;        
	ans = min(m["A"] + m["B"] + m["C"], ans);        
	ans = min(m["A"] + m["BC"], ans);        
	ans = min(m["B"] + m["AC"], ans);        
	ans = min(m["C"] + m["AB"], ans);        
	ans = min(m["AB"] + m["AC"], ans);        
	ans = min(m["BC"] + m["AC"], ans);        
	ans = min(m["AB"] + m["BC"], ans);        
	ans = min(m["ABC"], ans);        
	if(ans < 0 || ans >= INF) ans = -1;        
	cout << ans << endl; 
} 
```
