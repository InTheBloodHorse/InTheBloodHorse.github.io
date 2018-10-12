---
title: 每日一题(7) codeforces 1036C
date: 2018-09-17 12:14:19
tags:
- DFS
- 数位dp
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1036/problem/C)
题意：
找出区间 [A,B]内 非0的数字 不超过3的数字的个数。
4，200000，10203	 满足条件
4231，102306，7277420000 不满足条件
## 思路：
### dfs
可以用数位dp来做，我这里是用 dfs，找到的符合条件的数字 推到 vector里面，最后用差分的思想，用二分找到位置，相减就可以了。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
vector<long long>v;
			// 位数        数字        非0数字 
void dfs(int digit,long long number,int time)
{
	v.push_back(number);
	if(digit==18) return;
	dfs(digit+1,number*10,time);
	if(time<3){
		for(int i=1;i<=9;i++){
			dfs(digit+1,number*10+i,time+1);	
		}
	}
} 
int main()
{
	int t;
	for(int i=1;i<=9;i++){
		dfs(1,i,1);
	}
	// 记得把1e18也加上 
	v.push_back(1e18);
	sort(v.begin(),v.end());
	cin >> t;
	long long x,y;
	while(t--){
		cin >> x >> y;
				// 因为是闭区间，写法不唯一 
		cout << upper_bound(v.begin(),v.end(),y)-lower_bound(v.begin(),v.end(),x) << endl;
	}
}
 
```
### 数位dp
这道题用数位dp就是模版题，
solve:数位dp即可，记忆化一下,dfs传入三个变量:pos(数位)，num(当前还差几个数就能达到3个非0数),limit(是否到达上界)。
AC代码
```C
#include<bits/stdc++.h>
using namespace std;
int a[20];
long long dp[20][10];
long long dfs(int pos,int num,bool lim){
	if(pos<0 && num>=0) return 1;
	if(pos<0 || num<0) return 0;
	if(!lim && dp[pos][num]!=-1) return dp[pos][num];
	int up = lim ? a[pos]:9;
	long long ans=0;
	for(int i=0;i<=up;i++){
		if(i==0) ans+= dfs(pos-1,num,lim && i==a[pos]);
		else ans+= dfs(pos-1,num-1,lim && i==a[pos]);
	}
	if(!lim) dp[pos][num]=ans;
	return ans;
}
long long solve(long long x){
	int pos=0;
	while(x){
		a[pos++]=x%10;
		x/=10; 
	}
	return dfs(pos-1,3,true);
}
int main()
{
	int t;
	cin >> t;
	long long x,y;
	while(t--){
		cin >> x >> y;
		memset(dp,-1,sizeof(dp));
		cout << solve(y)-solve(x-1) << endl; 
	}
} 
```


