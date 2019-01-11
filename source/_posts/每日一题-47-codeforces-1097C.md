---
title: 每日一题(47) codeforces 1097C
date: 2019-01-09 21:49:01
tags:
- 字符串
- 栈
- 思维
categories:
- 每日一题
---
[题目地址](https://codeforces.com/contest/1097/problem/C)
## 题意
给你n个字符串，由()组成，要求把这些字符串两两拼接起来，使得满足()的规则数量最多。

## 思路
先预处理字符串，先把原字符串内符合条件的删除，最后会得到四种结果：
1.  (这是个空字符串)
2. ...))))) （n个右括号）
3. ...(((((  (n个左括号)
4. ...))))((((.....

第一种情况的话，只能和第一种情况合并，也就是第一种情况的数量/2
第二种情况只能和第三种情况中拥有相同数量的括号合并
第三种情况同理
第四种不可能拼接成符合规格的，可以忽略

其实这题难点不在实现，想法也不难，就是卡超时，空间换时间。

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
string s;
stack<char>temp;
int ans;
// l,r代表( )个数，ansl,ansr 代表 “情况”的个数 
int l[500005],ansl[500005];
int r[500005],ansr[500005];
void getString(int index){
	while(!temp.empty()) temp.pop();
	for(int i=0;i<s.size();i++){ 
		if(s[i]=='('){
			temp.push(s[i]);
			l[index]++;
		}
		else{
			if(temp.empty()){
				temp.push(s[i]);
				r[index]++;
				continue;
			}
			if(temp.top()=='(') {
				temp.pop();
				l[index]--;		
			}
			else{
				temp.push(s[i]);
				r[index]++;	
			}
		}
	} 
	if(l[index]==r[index] && temp.empty()){
		ans++;
	}else {
		if(r[index]==0 && l[index]!=0){
			ansl[l[index]]++;
		}else if(r[index]!=0 && l[index]==0){
			ansr[r[index]]++;
		}
	} 	
}

int main()
{
	int n;
	cin >> n;
	for(int i=0;i<n;i++){
		cin >> s;
		getString(i);
	}
	ans /= 2;
	for(int i=1;i<500005;i++){
		ans += min(ansl[i],ansr[i]);
	}
	cout << ans << endl;
} 
```


