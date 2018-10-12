---
title: 每日一题(21) codeforces 1047A
date: 2018-10-04 22:06:30
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/problemset/problem/1047/A)
## 题意
给一个数字n，拆成三个数字，这三个数字都不能被3整除
## 思路
拆成 1，2就行了，具体看代码。
```C
#include<bits/stdc++.h>
using namespace std;
int main(){
	int n;
	cin >> n;
	int mod = n%3;
	switch(mod){
		case 0:{
			cout << 1 <<" " <<  1 <<" " << n-2 << endl;
			break;
		};
		case 1:{
			cout << 1 <<" " <<  2 <<" " << n-3 << endl;
			break;
		};
		case 2:{
			cout << 2 <<" " <<  2 <<" " << n-4 << endl;
			break;
		};
	}
}
```

