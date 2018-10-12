---
title: 每日一题(26) codeforces 1051C
date: 2018-10-09 13:30:18
tags:
- 构造
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1051/problem/C)
## 题意
给一个数组，要求将这个数组分成 A，B两个数组，并且拥有相同的nice数（即这个数字在数组里只一个）
## 思路
1. 原数组里面有偶数个nice数，那么平分这些nice数。
2. 原数组里有奇数个，那么我们就要额外得到一个，可以从多的数字里拆出来，但是那个数字的数量要大于2，因为假如是两个的话，拆出来两个nice数了。大于2的话，拆成 1，x（总数-1）。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
struct node{
	int cnt;
	int index;
	string s;
}po[105];
int a[105];
int main()
{
	int n;
	cin >> n;
	for(int i=1;i<=100;i++) {
		po[i].cnt=0;
		po[i].index=0;
		po[i].s="";	
	}
	for(int i=0;i<n;i++){
		cin >> a[i];
		po[a[i]].cnt++;
	}
	int cnt=0;
	int flag=0;
	for(int i=1;i<=100;i++){
		if(po[i].cnt==1){
			cnt++;
		}
		if(po[i].cnt >= 3) flag=1;
	}
	if(cnt%2==1 && !flag){
		cout << "NO" << endl;
	}else{
		cout << "YES" << endl;
		int index;
		int mid = cnt/2;
		for(int i=0;i<n;i++){
			if(po[a[i]].cnt>=3) index=i;
		}
		for(int i=0;i<n;i++){
			if(i==index && cnt%2==1){
				continue;
			}
			if(po[a[i]].cnt==1 && mid){
				po[a[i]].s = po[a[i]].s + "A";
				mid--; 
			}else{
				po[a[i]].s = po[a[i]].s + "B";
			}
		}
		if(cnt%2==1){
			po[a[index]].s = "A";
			for(int i=1;i<po[a[index]].cnt;i++){
				po[a[index]].s = po[a[index]].s + "B";
			}
		}
		for(int i=0;i<n;i++){
			cout << po[a[i]].s[po[a[i]].index++];
		}
	}
} 
```
