---
title: 每日一题(34) codeforces1059C
date: 2018-10-17 13:33:04
tags:
- 构造
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1059/problem/C)
## 题意
给一个数字n，每次从这n中删除一个数字，删除前计算数字里面的gcd，问如何删除，可以使得gcd最大。
## 思路
因为相邻的两个数字是互质的，所以我们要删除奇数，因为当所有的奇数删完之后，每次计算的gcd肯定是大于等于2的。
奇数删完后，把奇数位置的数字当奇数，再筛选，输出，直到没有。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int n;
	cin >> n;
	int ans=1;
	while(n>3){
		for(int i=0;i<n/2+n%2;i++){
			cout << ans << " ";
		}
		n>>=1;
		ans<<=1;
	}
	if(n==3) cout << ans << " " << ans << " " << ans*3 << endl;
	else if(n==2) cout << ans  << " " << ans*2 << endl;
	else cout << ans << endl;
} 
```
