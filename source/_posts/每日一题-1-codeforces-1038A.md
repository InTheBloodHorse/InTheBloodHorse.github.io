---
title: 每日一题(1) codeforces 1038A
date: 2018-09-09 16:27:42
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1038/problem/A)
题意：说真的一开始看了老半天硬是没看懂，后面就看了Note，也是似懂非懂，后来瞎看就大致知道了。
就是给 n，k，给string s
求所有字母里出现次数最少的 * k
（这道题就是题意太坑了）

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int v[27];
int main()
{
	string s;
	int n,k;
	cin >> n >> k >> s;
	int min1 = 1e9;
	for(int i=0;i<s.size();i++){
		v[s[i]-'A'] ++;
	}
	for(int i=0;i<k;i++){
		if(v[i] < min1) min1 = v[i];
	}
	cout << min1*k << endl;
}
```