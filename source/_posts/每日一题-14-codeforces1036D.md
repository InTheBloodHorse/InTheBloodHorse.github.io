---
title: 每日一题(14) codeforces1036D
date: 2018-09-25 20:04:02
tags:
- 贪心
categories:
- 每日一题
---
[题目地址](http://codeforces.com/problemset/problem/1036/D)
题意：
给两串数字，每个数字可以和相邻的合并，问合并到最后，两串数字相同时候的最大长度为多少
思路：
从左到右匹配就好了，没有想的那么难，因为假如两串数字的sum不同的话 肯定是-1，所以答案最少为1，即所有加起来。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
long long a[300005],b[300005];
int main()
{
	int n,m;
	long long sum1=0,sum2=0;
	cin >> n;
	for(int i=0;i<n;i++){
		scanf("%lld",&a[i]);
		sum1+=a[i];
	}
	cin >> m;
	for(int i=0;i<m;i++){
		scanf("%lld",&b[i]);
		sum2+=b[i];
	}
	if(sum1!=sum2){
		cout << -1 << endl;
		return 0; 
	}
	sum1=0;sum2=0;
	int index1=0;
	int index2=0;
	int ans=0;
	while(index1<n && index2<m){
		if(sum1==sum2){
			sum2+=b[index2++];
			sum1+=a[index1++];
			ans++;
		}else if(sum1 > sum2){
			sum2+=b[index2++];
		}else{
			sum1+=a[index1++];
		}
	}
	cout << ans << endl;
}
```
