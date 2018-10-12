---
title: 每日一题(5)codeforces 1036A
date: 2018-09-13 20:03:01
tags:
- 数学
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1036/problem/A)
题意：（先看题目的图）给两个数字，n,k，要求图内（0~2n）组成的三角形面积之和等于k，的情况下，输出最小的高。
思路：
画个图就明白了。当h为1的时候，最大的面积为    底*高 /2  =  （2*n* 1）/2 = n
k在  (n * (h-1) , n * h]  (h >= 1 )
求 h 就可以了。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	long long a,b;
	cin >> a >> b;
	cout << (b-1)/a +1 << endl;
}
```

