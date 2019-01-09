---
title: 每日一题(46) codeforces 1097B
date: 2019-01-08 22:03:40
tags:
- DFS
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1097/problem/B)
## 题意
给n个数字，问这n个数字之间 或加 或减运算，能够使转盘的指针最终指向0
## 解法一：
好久没写搜索了。很是激动。
```C
void dfs(int index,int op,int sum)
```
index代表操作次数，op代表运算操作（- | +），sum代表当前指针位置

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
int a[1004];
int n;
string ans;
void dfs(int index,int op,int sum){
	sum = (sum+360)%360;
	if(index==n){
		if(sum==0){
			ans="YES";
		}
		return;
	}
	if(op==0){
		dfs(index+1,0,sum+a[index]);
		dfs(index+1,-1,sum+a[index]);
	}else{
		dfs(index+1,0,sum-a[index]);
		dfs(index+1,-1,sum-a[index]);
	}
}
int main()
{
	ans="NO";
	cin >> n;
	for(int i=0;i<n;i++){
		cin >> a[i];
	}
	dfs(0,0,0);
	cout << ans << endl;
 } 
```

## 解法二：
使用二进制枚举的方法，二进制为1代表 加操作，为0代表减操作。
AC代码：
```C
#include <bits/stdc++.h>
using namespace std;
int a[18];
int main()
{
    int n;
    cin >> n;
    for(int i=0;i<n;i++) cin >> a[i];
    for(int i = 0; i < (1<<n); i++) //从0～2^n-1个状态
    {
    	int ans = 0;
        for(int j = 0; j < n; j++) //遍历二进制的每一位
        {
        	ans = (ans+360)%360;
            if(i & (1 << j))//判断二进制第j位是否存在
            {
                ans += a[j];
            }else{
            	ans -= a[j];
			}
        }
        if(ans==0){
        	cout << "YES" << endl;
        	return 0;
		}
    }
    cout << "NO" << endl;
}
```
