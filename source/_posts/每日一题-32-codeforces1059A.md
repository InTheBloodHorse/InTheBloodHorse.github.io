---
title: 每日一题(32) codeforces1059A
date: 2018-10-15 21:12:43
tags:
- 模拟
categories:
- 每日一题
---
哇，周一太忙了，没午睡，现在困成狗。
[题目地址](http://codeforces.com/contest/1059/problem/A)
## 题意
一个人每天工作m分钟，有n个顾客，每个顾客a的时刻来，停留b分钟，然后他要接待每个顾客，他可以休息k分钟为一轮，问他一天可以休息几次。
## 思路
用数组来维护空闲的时间，我是用二个一组的，即[st,end]，看代码就明白了。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
vector<int>v;
int main()
{
	int n,m,k;
	int a,b;
	cin >> n >> m >> k;
	v.push_back(0);
	for(int i=0;i<n;i++){
		cin >> a >> b;
		v.push_back(a);
		v.push_back(a+b);
	}
	v.push_back(m);
	int sum=0;
	for(int i=0;i<v.size();i+=2){
		sum+= (v[i+1]-v[i])/k;
	}
	cout << sum << endl;
} 
```
