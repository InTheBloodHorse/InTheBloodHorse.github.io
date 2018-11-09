---
title: 每日一题(39) codeforces1054A
date: 2018-11-09 14:12:07
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1054/problem/A)
从10.20号写PHP项目，写到现在，也算是终于结束了，希望这辈子再也没有PHP，今天开始恢复训练。
## 题意
水题，但是题目很难读，读题需要点时间。大致就是输入 x,y,z,t1,t2,t3 从x楼到y楼，有两种方案
1：爬楼梯，每层需要的时间为t1
2：电梯，电梯一开始在z层，电梯一开始的门是关着的，电梯开门关门需要的时间为t3，电梯每层花费t2。
问改怎么选，选电梯（电梯优先）就输出YES，楼梯就NO。
## 思路
没啥思路
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int x,y,z,t1,t2,t3;
	cin >> x >> y >> z>> t1>>t2>>t3;
	int star = 0;
	star = abs(x-y) * t1;
	int eve = 0;
	eve = 3*t3 +  (abs(z-x) + abs(x-y)) * t2;
	if(eve<=star) cout << "YES" << endl;
	else cout << "NO" << endl; 	
} 
```
