---
title: 每日一题(48) codeforces1091A
date: 2019-01-11 19:11:57
tags:
- 思维
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1091/problem/A)
## 题意
给三个数字，abc，在不大于abc的数值下，输出abc为等差数列，差值为1的最大值，例如，13,5,6,答案为4+5+6

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main(){
	int a,b,c;
	cin >> a >> b >> c;
	cout << min(min(b,a+1),c-1) * 3 << endl;
} 
```
