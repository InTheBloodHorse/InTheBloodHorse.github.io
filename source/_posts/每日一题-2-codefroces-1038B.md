---
title: 每日一题(2) codefroces 1038B
date: 2018-09-10 19:33:57
tags:
- 素数
- 规律
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1038/problem/B)
今天的题目倒是很友好。
题意：
输入一个n，将1~n 分为两个集合，每个数字只能用一次，问能否找到两个区间，s1,s2
gcd(sum(s1),sum(s2)) > 1 ，换句话说就是 两个集合的总和 不能互质。
解题思路：
可以先打表，看到 sum 是有规律的，比如 1，3（1+2），6（1+2+3），10（1+2+3+4），15（1+2+3+4+5），四个一组，奇奇偶偶，这样子，偶数就很简单了，偶数-偶数，还是偶数，所有偶数的情况只要取出一个2，其他的是2的补集，就OK了。奇数的话，其实只要判断是不是素数就可以了，但是我这个素数的板子用反了，应该是用判断这个数是不是素数的板子，不过问题不大，开了2s，是素数就是No，不是素数的话，一个集合是 可以整除sum（1~n）的数，另一个就是补集了。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int f[20000]={2};
	int k=1;
	for(int i=3;i<45000;i++)
	{
		int q=1;
		for(int j=0;j<k;j++)
		{
			if(i%f[j]==0){q=0;break;}	
		}
		if(q) f[k++]=i;	
	}
	int n;
	cin >> n;
	if(n==1){
		cout << "No" << endl;
		return 0;
	}
	if(n%4==0 || n%4==3){
		cout << "Yes" << endl;
		cout << "1 2" << endl;
		cout << n-1;
		for(int i=1;i<=n;i++){
			if(i!=2) cout << " " << i;
		}
		cout << endl;
	}else{
		long long sum=n*(n+1)/2;
		int index=0;
		while(f[index]<sum){
			if(sum%f[index]==0){
				cout << "Yes" << endl;
				cout << 1 << " " << f[index] << endl;
				cout << n-1 ;
				for(int i=1;i<=n;i++){
					if(i!=f[index]) cout << " " << i;
				}
				cout << endl;
				return 0;
			}else index++;
		}
		cout << "No" << endl;
	}
}
 
```
