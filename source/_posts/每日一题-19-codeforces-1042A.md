---
title: 每日一题(19) codeforces 1042A
date: 2018-09-30 21:04:04
tags:
- 优先队列
categories:
- 每日一题
---
[题目地址](http://codeforces.com/problemset/problem/1042/A)
## 题意
输入n，m代表n个长椅，还要加入的人数。
接下来n行输入每个长椅已有的人数
输出安排这m个后，长椅最多人数里面的 最少的人数为多少，最多的人数为多少。
## 思路：
最多的人数很简单
最少的去模拟坐椅子，用优先队列来维护，每次把人安排到当前人最少的椅子。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[200];

priority_queue<int,vector<int>,greater<int> > q;
int main()
{
	int n,m,x;
	cin >> n >> m;
	int maxn=0; 
	for(int i=0;i<n;i++){
		cin >> x;	
		maxn = max(maxn,x);
		q.push(x);
	}
	
	for(int i=0;i<m;i++)
	{
		int top = q.top();
		q.pop();
		q.push(top+1);
	}
	int ans=0;
	for(int i=0;i< n;i++)
	{
		ans = max(q.top(),ans);
		q.pop();
	}
	cout << ans << " " << maxn+m << endl; 
	 
} 
```

