---
title: 每日一题(29)codeforces 1058C
date: 2018-10-12 14:55:22
tags:
- 构造
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1058/problem/C)
## 题意
给n个数字，每个数字只能用一次，并且有序，问你能否拆成 2或更多段，使得每段的位数总和相同。
## 思路
因为最多100个数字，暴力一下就好了。一开始题目读错了，还以为每次拆出来的数字序列要非递增（瞎了瞎了）。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[10001];
int main()
{
	int n;
	cin >> n;
	string s;
	cin >> s;
	int sum=0;
	for(int i=0;i<n;i++){
		a[i]=s[i]-'0';
		sum+=a[i];
	}
	for(int i=2;i<=n;i++){
		if(sum%i==0){
			vector<int>v;
			v.clear();
			int need=sum/i;
			int index=-1;
			int cur_sum=0;
			for(int j=0;j<n;j++){
				cur_sum+=a[j];
				if(cur_sum==need){
					cur_sum=0;
					v.push_back(index);
					index=j;
				}else if(cur_sum>need){
					break;
				}
			}
			if(v.size()==i){
				cout << "YES"<< endl;
				return 0; 
			}
		}
	}
	cout << "NO" << endl;
} 
```

