---
title: 每日一题(6) codeforces 1036B
date: 2018-09-14 19:58:33
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1036/problem/B)
题意：
给三个数,x,y,k,代表从 (0,0)到(x,y)走k步(米字形走法)，问可以走的对角线最大步数有多少步。
思路：
想了很久，难受啊。
abs(x-y)为奇数时，不能走对角线，需要多走一步k-1，当走对角线可以直接到达终点，如果剩余的步数是奇数则有两步不能走对角线所以k - 2
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int t;
	cin  >> t;
	long long x,y,k;
	while(t--){
		cin >>  x >> y >> k;
		if(max(x,y) > k){
			cout << -1 << endl;
		}else{
			long long ans = k;
			if(abs(x-y)%2==1){
				ans--;
			}else{
				if((k-x)%2 == 1){
					ans-=2;
				}
			}
			cout << ans << endl;
		}
	}
} 
```
