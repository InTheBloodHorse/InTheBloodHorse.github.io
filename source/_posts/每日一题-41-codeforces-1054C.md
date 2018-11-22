---
title: 每日一题(41) codeforces 1054C
date: 2018-11-22 18:48:29
tags:
- 构造
- 思维
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1054/problem/C)
## 题意
输入n个学生，吗，学生挨个坐在一起，已知每个学生左边比他苹果多的人数，以及右边比他苹果多的人数，输出每个学生的苹果数，假如不能得出，则输出NO

## 思路
大致可以发现，l+r的个数相同的话，他们的苹果个数也相同。然后先构造出数组，然后检查一下就好了。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;

int l[1005],r[1005];
int num[1005];
int main()
{
	int n;
	cin >> n;	
	for(int i=0;i<n;i++){
		cin >> l[i];
	}
	vector<int>v;
	for(int i=0;i<n;i++){
		cin >> r[i];
		num[i]=r[i]+l[i];
		v.push_back(n-num[i]);
	}
	for(int i=0;i<n;i++){
		int left=0;
		int right=0;
		for(int j=0;j<i;j++){
			if(v[j] > v[i]) left++;
		}
		if(left!=l[i]) return puts("NO");
		
		for(int j=i+1;j<n;j++){
			if(v[j] > v[i]) right++;
		}
		if(right!=r[i]) return puts("NO");
	}
	cout << "YES" << endl;
	for(int i=0;i<n;i++){
		cout << v[i] << " ";
	}
	cout << endl;
} 
```


