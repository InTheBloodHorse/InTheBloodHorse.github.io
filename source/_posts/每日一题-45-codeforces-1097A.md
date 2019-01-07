---
title: 每日一题(45) codeforces 1097A
date: 2019-01-07 16:15:23
tags:
- map
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1097/problem/A)
## 题意
给一个目标字符串AB，接下来给五个字符串XY，假如X=A || Y=B输出YES

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
map<char,int>m1;
map<char,int>m2;
int main()
{
	string ans = "NO";
	string need;
	cin >> need;
	string ff;
	m1[need[0]] = 1;
	m2[need[1]] = 1;
	for(int i=0;i<5;i++){
		cin >> ff;
		if(m1[ff[0]] == 1) ans="YES";
		if(m2[ff[1]] == 1) ans="YES";
	} 
	cout << ans << endl;
}
```
