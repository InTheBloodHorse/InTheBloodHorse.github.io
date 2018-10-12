---
title: 每日一题(16) codeforces 1041B
date: 2018-09-27 18:48:41
tags:
- gcd
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1041/problem/B)
题意：
问 在[1,x],[1,y]，比例 等于 a:b的个数
思路：
把a:b的比例约分，这样就能保证最小，其实就是除以 gcd(a,b)。
然后除以比例就好了，找出最小的。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
long long gcd(long long a, long long b) {
  return b == 0 ? a : gcd(b, a % b);
}
int main()
{
	long long x,y,a,b;
	cin >> x >> y >> a >> b;
	long long div = gcd(a,b);
	a/=div;
	b/=div;
	cout << min(x/a,y/b) << endl;
}
```
