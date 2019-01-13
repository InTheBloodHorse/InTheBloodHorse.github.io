---
title: 每日一题(49) codeforces 1091B
date: 2019-01-13 21:27:44
tags:
- map
- STL
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1091/problem/B)
## 题意
给n个坐标以及n个向量，坐标与向量组合可以得到一个目的坐标，要求输出每个坐标与任意一个向量组合都可以得到的目的坐标的坐标。（可能有多个，随意输出哪个）

## 思路
n最大为1000，所以可以暴力求解。暴力将每一个坐标与每一个向量进行组合，并计数，计数为n的坐标就是目标坐标了。

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int n;
map<pair<int,int>,int>m;
vector<pair<int,int> >v1;
vector<pair<int,int> >v2;
int main()
{
	cin >> n;
	int x,y;
	for(int i=0;i<n;i++){
		cin >> x >> y;
		v1.push_back(make_pair(x,y));
	}
	for(int i=0;i<n;i++){
		cin >> x >> y;
		v2.push_back(make_pair(x,y));
	}
	for(int i=0;i<v1.size();i++){
		x=v1[i].first;
		y=v1[i].second; 
		for(int j=0;j<v2.size();j++){
			m[make_pair(x+v2[j].first,y+v2[j].second)]++;
		} 
	}
	map<pair<int,int>,int>::iterator iter;
	for(iter=m.begin();iter!=m.end();iter++){
		if(iter->second==n){
			cout << iter->first.first << " " << iter->first.second << endl;
			return 0;
		}
	}
} 
```
