---
title: 每日一题(37) codeforces1064C
date: 2018-10-20 20:08:20
tags:
- 构造
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1064/problem/C)
## 题意
给你一个字符串，元素不变，构造成新的字符串，使得字符串的子串中是回文串的数量最多。
## 思路
排序一下就行了。
```C
#include<bits/stdc++.h>
using namespace std;
char s[100005];
int main()
{
	int n;
	cin >> n;
	scanf("%s",&s);
	sort(s,s+n);
	printf("%s",s);
}
```

