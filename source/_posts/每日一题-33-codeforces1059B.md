---
title: 每日一题(33) codeforces1059B
date: 2018-10-16 09:29:57
tags:
- 模拟
- 暴力
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1059/problem/B)
## 题意
有一个印章的形状为 
```Bash
###
#.#
###
```
给你一个形状，问能否用这个印章，印成给定的形状。
## 思路
暴力就可以了，寻找周围都是#的点，说明这里必须要被印，找完之后，和给的形状比较一下，看下是否相同
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
string ma[1005];
int n,m;
int my[1005][1005];
//判断能否印 
int judge(int x,int y){
	for(int i=x-1;i<=x+1;i++){
		for(int j=y-1;j<=y+1;j++){
			if(i==x && j==y)continue;
			if(ma[i][j]=='.') return 0;
		}
	}
	return 1;
}
//印 
void done(int x,int y){
	for(int i=x-1;i<=x+1;i++){
		for(int j=y-1;j<=y+1;j++){
			if(i==x && j==y)continue;
			my[i][j]=1;
		}
	}
}
int main()
{
	cin >> n >> m;
	for(int i=0;i<n;i++){
		cin >> ma[i]; 
	}
	for(int i=1;i<n-1;i++){
		for(int j=1;j<m-1;j++){
			if(judge(i,j)){
				done(i,j);
			}
		} 
	}
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if((my[i][j]==1 && ma[i][j]=='.') ||(my[i][j]==0 && ma[i][j]=='#')){
				cout  << "NO" << endl;
				return 0;
			}
		}
	}
	cout << "YES" << endl;
} 
```

