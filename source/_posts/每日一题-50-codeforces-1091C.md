---
title: 每日一题(50) codeforces 1091C
date: 2019-01-15 22:21:28
tags:
- 数论
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1091/problem/C)

## 题意
给一个数组n，代表一个环有n个节点，从1到n，每次可以跳一个步数，比如跳1步的话，经过的点就是 [1,2,3,...,n]，跳2步的话，经过的点为 [1,3,5,...]。要求升序输出能跳回起点（1）的步数总和。

## 思路
1. 数据集很大，模拟肯定不行
2. 可以发现 答案序列长度和因数有关，先暴力找出因子，然后根据等差数列求和，得出答案序列

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
vector<long long>v;
int main()
{
	int n;
	cin >> n;
	for(int i=1;i<=sqrt(n);i++){
		if(n%i==0){
			v.push_back(1LL*i);
			if(n/i != i) v.push_back(1LL* (n/i));
		} 
	}	
	sort(v.begin(),v.end());
	for(int i=v.size()-1;i>=0;i--){
		int number = n/v[i];
		cout << number + v[i]*(number-1)*number/2 << " "; 
	} 
} 
```



