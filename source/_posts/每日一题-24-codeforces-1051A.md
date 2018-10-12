---
title: 每日一题(24) codeforces 1051A
date: 2018-10-07 20:01:10
tags:
- 构造
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1051/problem/A)
## 题意
给出一个字符串，如何修改某些字符可以达到要求，即这些字符串里必须有大写小写数字。输入也只有大写小写数字。输出修改后的字符串，但是要求修改的位置的长度为最小。例如 从 abcdef->a7cdEf，的长度为（5-2）+1=4。
## 思路：
首先只会出现大写，小写，数字，所以只会出现三中情况。
1. 大写，小写，数字都有了，直接输出
2. 少了一个（大写|小写|数字），只要随便找个不是唯一类别的修改成数字就行了。因为长度永远为1。
3. 少了两个，说明这个字符串要么全都是数字，要么全都是大写的，或者小写的。只要相邻的一起改了就行了。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int d[1000],u[1000],l[1000];
int main()
{
	int n;
	cin >> n;	
	string s;
	for(int i=0;i<n;i++){
		int low=-1,upp=-1,dig=-1;
		int cnt_d=0;
		int cnt_lower=0;
		int cnt_upper=0;
		memset(d,0,sizeof(d));
		memset(l,0,sizeof(l));
		memset(u,0,sizeof(u));
		cin >> s;
		for(int j=0;j<s.size();j++)
		{
			if(s[j]>='0' && s[j]<='9'){
				dig=j;
				d[++cnt_d]=j;	
			}
			if(s[j]>='a' && s[j]<='z'){
				low=j;
				l[++cnt_lower]=j;		
			}
			if(s[j]>='A' && s[j]<='Z'){
				upp=j;
				u[++cnt_upper]=j;	
			}
		}
		if(low == -1) {
			if(cnt_upper >= 2) s[u[cnt_upper--]]='a';			
			else s[d[cnt_d--]] = 'a';		
		}
		if(upp == -1) {		
			if(cnt_lower>=2) s[l[cnt_lower--]] = 'A';			
			else s[d[cnt_d--]] = 'A';		
		}		
		if(dig == -1) {			
			if(cnt_lower>=2) s[l[cnt_lower--]] = '1';			
			else s[u[cnt_upper--]] = '1';		
		}

		cout << s << endl;
	}
} 
```

