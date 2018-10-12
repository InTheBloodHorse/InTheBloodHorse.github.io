---
title: 每日一题(22)codeforces 1047B
date: 2018-10-05 18:08:54
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1047/problem/B)
题意：
大水题，好久没这么舒服了，给出n个坐标，求x,y轴的最大值。
思路：
不用思路
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int ans=0;
	int n;
	cin >> n;
	int a,b;
	for(int i=0;i<n;i++)
	{
		cin >> a >> b;
		ans = max(ans,a+b);
	}	
	cout << ans << endl;
} 
```
