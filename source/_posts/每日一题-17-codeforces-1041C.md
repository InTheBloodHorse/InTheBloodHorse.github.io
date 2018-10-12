---
title: 每日一题(17) codeforces 1041C
date: 2018-09-28 20:19:55
tags:
- 优先队列
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1041/problem/C)
题意：给三个数字，n,m,d
n：给n个数字
m：表示这n个数字在[1,m]内(说实话我也没发现m是干嘛用的)
d：差值(差值的意思后面会讲)
要求给这n个数字分组，分组的个数尽可能的少。每组个数从小到大 相邻的位置差值不能 <= d
输出分组的个数
以及这n个数字在哪个分组里面
思路：
先对n个值进行排序，然后从小到大，模拟排队，符合条件的话，就插入队列，假如不符合，就新开一个队列，到最后，输出队列的个数，其中记录下每个数字所在的队列的坐标。
但是！超时了，因为我暴力去找是否符合能插入队列，这里应该用优先队列来维护，这样查找的复杂度就从 O(m) 降低到了 O(log m) m为当前队伍的个数,这样就不会超时了。
所以我们每次待插入的数字和优先队列的队头元素比较就好了，因为我们已经排序过了，所以我们待插入的数据肯定是比优先列队的对头元素要大，关键是大多少了。
AC代码：
```C
#include<bits/stdc++.h>
using namespace std;

struct node{
	int x;
	int index;
}po[200005];

map<int,int> m;
priority_queue<int,vector<int>,greater<int> > q;
int cmp(node a,node b){
	return a.x < b.x;
} 
int cmp_index(node a,node b){
	return a.index < b.index;
} 
int main()
{
	int n,m1,d;
	cin >> n >> m1 >> d; 
	for(int i=0;i<n;i++){
		cin >> po[i].x;
		po[i].index = i;
	}
	sort(po,po+n,cmp);
	m[po[0].x]=1;
	int num=1;
	q.push(po[0].x);
	for(int i=1;i<n;i++){
		int t_num = q.top();
		if(po[i].x - t_num > d){
			m[po[i].x] = m[t_num];
			q.pop();
		}else{
			m[po[i].x] = ++num;
		}
		q.push(po[i].x);
	}
	sort(po,po+n,cmp_index);
	cout << num << endl;
	for(int i=0;i<n;i++){
		cout << m[po[i].x] << " ";
	}
	cout << endl;
} 
```




