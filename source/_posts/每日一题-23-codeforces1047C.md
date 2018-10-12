---
title: 每日一题(23) codeforces1047C
date: 2018-10-06 18:54:27
tags:
- 素数
- gcd
categories:
- 每日一题
---
[题目地址](http://codeforces.com/problemset/problem/1047/C)
题意：
给n个数字，要求你删掉一些数字，使得剩下的数组的最大公因数要比没有删除之前的大。否则，输出-1
思路：
先找出当前数组的最大公因数，每个数字/最大公因数得到新的数组。然后统计数字出现最多的质因数x。答案就是 n - x。(其实这题多半看题解，不过收获在于我拿到了一个更好的素数模版！！！)
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int gcd(int a,int b)
{
	return b==0 ? a:gcd(b,a%b);
}

int vis[15000005];
int prime[15000005];
int minprime[15000005];
int cnt[15000005];
int k=0;
void sieve(int n) {
	for(int i=2; i<=n; i++) {
		if(!vis[i]) {
			minprime[i]=i;
			prime[++k]=i;
		}
		for(long long j=1; j<=k&&(long long)i*prime[j]<=n; j++) {
			minprime[(long long)i*prime[j]]=prime[j];
			vis[(long long)i*prime[j]]=1;
			if(!(i%prime[j])) break;
		}
	}
}
void div(int x)
{
	if(x==1 || !vis[x])
	{
		cnt[x]++;
		return;
	}
	while(x>1)
	{
		int y = minprime[x];
		cnt[y]++;
		while(x%y==0 && x!=-1) x/=y;
	}
}
vector<int>v;
int main()
{
	sieve(15000000);
	int n;
	cin >> n;
	int number;
	int min_gcd=0;
	for(int i=0;i<n;i++)
	{
		cin >> number;
		v.push_back(number);
		min_gcd = gcd(min_gcd,number);
	}
	int len=v.size();
	for(int i=0;i<len;i++)
	{
		v[i]/=min_gcd;	
		div(v[i]);
	}
	int maxn=0;
	for(int i=2;i<=15000000;i++) maxn = max(maxn,cnt[i]);
	if(maxn==0) cout << -1 << endl;
	else cout << n-maxn << endl;
} 
```
