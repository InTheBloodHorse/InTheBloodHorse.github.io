---
title: '每日一题(30) codeforces #515训练赛'
date: 2018-10-13 18:06:42
tags:
- 数学
- 思维
- 贪心
categories:
- 每日一题
---
我还以为这次的周练会很简单，因为我挑了div3的题目，打完才知道，这个div3写出一题就能加分了，太恐怖了，我觉得比div2还难。哭了。
# A
[题目地址](http://codeforces.com/contest/1066/problem/A)
## 题意
给a，b，c，d，代表总路长,速度，[c，d]排除的区间。要求给出从0点出发，b的速度，有几个点(排除区间[c，d])
#思路
简单数学题，除一下就好了。
```C
#include<bits/stdc++.h>
using namespace std;
int main(){
	int t;
	cin >>t;
	int a,b,c,d;
	while(t--){
		cin >>a>>b >>c >>d;
		cout << a/b - (d/b - (c-1)/b) << endl;
	}
}	 
```
# C
[题目地址](http://codeforces.com/contest/1066/problem/C)
## 题意
有 L，R，?三种操作，L，R分别代表从左插入，从右插入，?代表输出对应的数字最小值（从左往右数，或者从右到左数的最小位置）
## 思路
每次记录下l，r指针，然后用 value->index的方式记录，比赛的时候一直用index->value方式记录，很蠢，还一直wa，黄佬还一直TLE，哈哈哈哈
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[200005];
int main()
{
	int n;
	cin >> n;
	int l=0,r=0;
	string s;
	int t;
	for(int i=0;i<n;i++){
		cin >> s >> t;
		if(i==0){
			a[t]=r;
			r++;
			l--;
			continue;
		}
		if(s=="L") a[t]=l--;
		else if(s=="R") a[t]=r++;
		else cout << min(abs(a[t]-l),abs(a[t]-r))-1 << endl;
	}
}
```
# D
[题目地址](http://codeforces.com/contest/1066/problem/D)
## 题意
一共有n个盒子，容量为m，并给出物品的体积。要求放入的体积编号连续，还要尽量的多，而且假如发现放不下之后，就把最左边的盒子倒出（为空），重新放（还是看原题吧，翻译不出来）。
## 思路
因为无论如何怎么放，肯定是最后一批物品放进盒子里的，所以应该从后往前放。贪心就好了。但是我不知道为什么题目的标签是二分，为什么codeforces上才过了700多。应该是被这个D题吓坏了吧。
```C
#include<bits/stdc++.h>
using namespace std;
int a[2000005];
vector<long long>v[200005];
int main()
{
	int n,m,t;
	cin >> n >> m >> t;
	for(int i=0;i<n;i++){
		cin >> a[i];
	}
	int index=0;
	long long sum=0;
	for(int i=n-1;i>=0;i--){
		if(sum+a[i]<=t){
			sum+=a[i];
			v[index].push_back(a[i]);
		}else{
			if(index==m-1) break; 
			v[++index].push_back(a[i]);
			sum=a[i];
		}
	}
	int ans=0;
	for(int i=0;i<=index;i++){
		ans+=v[i].size();
	}
	cout << ans << endl;
}
```

