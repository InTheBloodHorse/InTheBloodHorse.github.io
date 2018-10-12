---
title: 每日一题(3) codeforces 1038C
date: 2018-09-11 20:28:34
tags:
- 排序
- 贪心
- 模拟
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1038/problem/C)
题意：
A,B两人，分别有list，有n个元素，A，B轮流行动，每人有两种选择：
1：移除对方的一个元素
2：移除自己的一个元素，并且自己的分数加上那个元素的分值
假设两个人都做出最优的前提下，A,B的分值之差（A-B）
思路：
一开始还以为是一道博弈题目，后面读题目就知道，只要先排序，然后轮流模拟A，B的操作，到最后就可以得出答案。
坑点：1e5*1e6 = 1e11 分数定义成int的话 就爆掉了，留个心眼。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
vector<int>v1,v2;
int main()
{
	int n;
	cin >> n;
	int a;
	for(int i=0;i<n;i++) 
	{
		cin >> a;
		v1.push_back(a);
	}
	for(int i=0;i<n;i++) 
	{
		cin >> a;
		v2.push_back(a);
	}
	sort(v1.begin(),v1.end());
	sort(v2.begin(),v2.end());
	long long sum1 =0,sum2=0;
	for(int i=0;i<2*n;i++)
	{
		int x,y;
		if(v1.size()==0) x=0;
		else x = v1[v1.size()-1];
		if(v2.size()==0) y=0;
		else y = v2[v2.size()-1];
		if(i%2==0){
			if(x>=y){
				sum1+=x;
				v1.pop_back();
			}else{
				v2.pop_back();
			}
		}else{
			if(y>=x){
				sum2+=y;
				v2.pop_back();
			}else{
				v1.pop_back();
			}
		}
	}
	cout << sum1 - sum2 << endl;
}
```
