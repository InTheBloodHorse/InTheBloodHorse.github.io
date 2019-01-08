---
title: 每日一题(46) codeforces 1097B
date: 2019-01-08 22:03:40
tags:
- DFS
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1097/problem/B)
## 题意
给n个数字，问这n个数字之间 或加 或减运算，能够使转盘的指针最终指向0
## 解法一：
好久没写搜索了。很是激动。
```C
void dfs(int index,int op,int sum)
```
index代表操作次数，op代表运算操作（- | +），sum代表当前指针位置

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[1004];
int n;
string ans;
void dfs(int index,int op,int sum){
	sum = (sum+360)%360;
	if(index==n){
		if(sum==0){
			ans="YES";
		}
		return;
	}
	if(op==0){
		dfs(index+1,0,sum+a[index]);
		dfs(index+1,-1,sum+a[index]);
	}else{
		dfs(index+1,0,sum-a[index]);
		dfs(index+1,-1,sum-a[index]);
	}
}
int main()
{
	ans="NO";
	cin >> n;
	for(int i=0;i<n;i++){
		cin >> a[i];
	}
	dfs(0,0,0);
	cout << ans << endl;
 } 
```

