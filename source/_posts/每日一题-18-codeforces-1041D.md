---
title: 每日一题(18) codeforces 1041D
date: 2018-09-29 20:45:57
tags:
- 思维
categories:
- 每日一题
---
[题目地址](http://codeforces.com/contest/1041/problem/D)
## 题意
给 n，m 分别代表有n个区域，m表示高度，当降落到这n个区域内，人的高度不变，只滑行，问人能够滑行的最大距离为多少。
## 思路
心情复杂，可以用滑块的思想来解决，但是卡了，哭了。
换一个思路，用整体的思想。
1：我们的下落点，必须在某个区域的最左边。
2：然后我们找到符合落地点的最后一个区域，这里要注意落地了就不能滑行了，这个临界值要注意。
3：根据上面两条，我们就可以用整体的思想来解决，因为我们不用关注其中的路线是怎么样的。
只要知道起始点和结束点，这是一段距离 我们记为 x，因为是等腰直角三角形，所以下滑的高度是等于滑行的距离的。
还有一段是 最后一段滑行后下降的距离，我们记为 y，为什么肯定有下滑，因为不存在在地面滑行的情况，具体看第二条。
我们的答案就是 x+y 里面的最大值。

AC代码：
```C
#include<bits/stdc++.h>
using namespace std;
struct node
{
	int x,y;
}po[200005];
vector<int>v;
int main()
{
	int n,m;
	cin >> n >> m;
	v.push_back(0);
	for(int i=0;i<n;i++){
		cin >> po[i].x >> po[i].y ;
		if(i!=0){
			v.push_back(v[i-1]+po[i].x - po[i-1].y);
		}
	}
	int ans=0;
	for(int l=0,r=0;l<n;l++)
	{
		while(v[r+1]-v[l]<m && r+1<v.size()) r++;
		int len = po[r].y - po[l].x+(m-(v[r]-v[l]));
		ans = max(ans,len);
	}
	cout << ans << endl;
} 
```
