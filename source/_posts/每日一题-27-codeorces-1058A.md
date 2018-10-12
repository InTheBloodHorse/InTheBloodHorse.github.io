---
title: 每日一题(27) codeorces 1058A
date: 2018-10-10 21:10:09
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1058/problem/A)
太简单了。不说了，写Spring去了。
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int flag=0;
	int n;
	cin >> n;
	int t;
	for(int i=0;i<n;i++)
	{
		cin >> t;
		if(t) flag=1;
	}
	if(flag) cout << "HARD" << endl;
	else cout << "EASY" << endl;
 } 
```

