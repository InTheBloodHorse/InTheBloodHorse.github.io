---
title: 每日一题(28) codeforces1058B
date: 2018-10-11 21:08:27
tags:
- 数学
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1058/problem/B)
## 题意
给n，d，可以得出一个矩形，接下来给出若干个点，问该点是否在这个矩形内。
## 思路
用线性规划，满足所有条件即可(高中毕业两年了，真的忘了)。
![TIM截图20181011211243.jpg](TIM截图20181011211243.jpg)
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int n,d;
	int t;
	cin >> n >> d;
	cin >> t;
	int x,y;
	for(int i=0;i<t;i++){
		cin >> x >> y;
		if(x+y-d<0||x+y-2*n+d>0||x-y-d>0||x-y+d<0) cout << "NO" << endl;
		else cout << "YES" << endl;
	}
} 
```
