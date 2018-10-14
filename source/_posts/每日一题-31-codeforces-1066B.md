---
title: 每日一题(31) codeforces 1066B
date: 2018-10-14 19:45:31	`
tags:
- 贪心
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1066/problem/B)
比赛的时候，我1Wa了，然后给黄佬切了。今天补回来。
## 题意
有n个房间，1代表有暖炉，0代表没有。问至少点亮多少暖炉，使得每个房间都温暖，每个暖炉都有范围m。
## 思路
贪心，找到每次点亮暖炉之后，从最近一个没有点亮的房间跳m，跳到下一个最远的位置，然后从最远的位置从右往左找，找到第一个暖壶，因为这样可以使暖壶的温暖位置尽量不会重叠。假如没找到。真的太难描述了。哭了。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[1005];
int vis[1005];
int main()
{
	int n,m;
	cin >> n >> m;
	for(int i=0;i<n;i++) cin >> a[i];
	int pos=-1;
	int ans=0;
	int pre=0;
	int sum=0;
	while(1){
		int next = pos+m;
		next = min(next,n-1);
		int flag=1;
		for(int i=next;i>=pre;i--){
			
			if(a[i]==1){
				a[i]=2;
				flag=0;
				ans++;
				for(int j=max(0,i-(m-1));j<=min(i+(m-1),n-1);j++){
					if(vis[j]==0){
						vis[j]=1;
						sum++;	
					}
					if(sum==n){
						cout << ans << endl;
						return 0;
					}
				} 
				pos=i+m-1;
				pre=i;
				break;
			}
		}
		if(flag){
			break;
		}
	}
	for(int i=0;i<n;i++) if(vis[i]==0){
		cout << -1 << endl;
		return 0;	
	} 
}
```

