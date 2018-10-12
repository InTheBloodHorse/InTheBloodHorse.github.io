---
title: 每日一题(4) codeforces 1038D
date: 2018-09-12 21:04:50
tags:
- 思维
- 排序
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1038/problem/D)
题意：
有n个史莱姆，每个史莱姆有值为 x，史莱姆A可以吃掉任意一个相邻位置的史莱姆B（值为y），因此史莱姆B就消失了，史莱姆A的值变成了 x-y。问一直吃，吃到最后一个，留下来的史莱姆最大的值为多少。
思路：
一开始以为是dp题，被吓坏了，后来琢磨下可以得出
1：最大的肯定是留到最后的，最小的必须去吃其余的，因为一个大的史莱姆去吃小的史莱姆，就亏了，因为大的史莱姆的值小了，但是他还是正数，最大的吃这个史莱姆，也会变小，所以要想办法得到负数的史莱姆
2：所有值都大于0的话，答案就是 所有数字之和 - 2*min
3：所有的值小于0的话，取个绝对值，和大于0的情况一样，自己在纸上写一下就知道为什了。
4：有大于0小于0，让负数去吃掉正数的，然后正数的吃掉负数的，就是 sum+=abs(value)
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
long long a[500005];

int main()
{
	int n;
	cin >> n;
	int t;
	for(int i=0;i<n;i++) cin >> a[i];
	if(n==1){
		cout << a[0] << endl;
	}else if(n==2){
		sort(a,a+n);
		cout << a[1]-a[0] << endl;
	}else{
		long long ans=0;
		int cnt=0;
		for(int i=0;i<n;i++){
			if(a[i]<0){
			 a[i]=-a[i];
			 cnt++;	
			}
			ans+=a[i];
		}
		sort(a,a+n);
		if(cnt==0 || cnt==n) ans-=2*a[0];
		cout << ans << endl;
	}
}
```
