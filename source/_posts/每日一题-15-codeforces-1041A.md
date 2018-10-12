---
title: 每日一题(15) codeforces 1041A
date: 2018-09-26 17:50:09
tags:
- 贪心
- 排序
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1041/problem/A)
题意：给N个数字，找出还要最少插入几个数字可以让这些数字连续
思路：
排序，贪心就好了
```C
#include<bits/stdc++.h>
using namespace std;
int a[2000];
int main()
{
	int n;
	cin >> n;
	for(int i=0;i<n;i++) cin >> a[i];
	sort(a,a+n);
	int ans=0;
	for(int i=1;i<n;i++){
		ans+=a[i]-a[i-1];
	}
	cout << ans-n+1 << endl;
}
```

