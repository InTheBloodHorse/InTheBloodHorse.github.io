---
title: 每日一题(40) codeforces1054B
date: 2018-11-12 18:09:47
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1054/problem/B)
## 题意
这个Mex一年前才杭电oj写过，我记得我是没写起来，这个Mex是个什么概念？就是在一个序列中找到一个从最小的没有的数字，比如序列[0,1,2,4] Mex下就是3，因为0没有。
这道题是意思是给n个数字，接下来是n个数字，问能否在当前的序列找到任意子集，可以得到输入的Mex数字，假如都能找到，就输出-1，找不到就输出一个找不到的index坐标。
比如 Examples1
4
0 1 2 1
第一个数字是0，那么我们取空集，Mex一下得到0，当前的序列就是[0]
然后要找数字1，直接去当前序列[0]然后我们就可以得到1，组成的序列为[0,1]。
同理也可以得到 [0,1,2]
第四个也可以得到1，因为我们取0就好了。
## 思路
暴力去找肯定不行，因为数据量太大。
step1：可以倒过来想，假设当前n个数字。最理想的情况下为0...n，所以可以得到最后一个（或者说当前的，Mex肯定<=n）。
step2：Mex(a[i])<=n没错，但是假如 序列为[0,0,1]，当我们计算第三次的时候很显然Mex出来的数字在0~2之间，也就是说，每次Mex出来的数字肯定是 <= 上一次Mex出来的数字 + 1。所以我们每次判断一下就好了
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
map<int,int>m;
int main()
{
	int n;
	cin >> n;
	int ans = -1;
	int t;
	int cur=0; 
	for(int i=0;i<n;i++){
		cin >> t;
		if(t>cur){
			cout << i + 1<< endl;
			return 0;
		}
		m[t]++;
		if(m[t]==1) cur++;
	}	
	cout << -1 << endl;
} 
```
