---
title: 数据结构与算法(10) KMP算法
date: 2019-01-12 12:39:01
tags:
- KMP
- 字符串
categories:
- 数据结构与算法
---
## 什么是KMP
其实在大一下暑假就接触到了KMP，那时候真心看不懂，后续在日常训练中，也几乎遇不到KMP的题目，所以KMP在很长一段时间就被搁置了，只知道KMP里面有一个叫next数组。现在重新捡回来。KMP是解决在字符串A中查找字符串B的下标的问题，即字符串的匹配过程，假如找到字符串B，返回下标，否则返回-1。
## 为什么选择KMP
查找子串里，最容易想到的就是暴力枚举了，假设字符串A长度为n，要查找的字符串B长度为m，这时候的时间复杂度为 n \* m，在数据规模很大的情况下就显得不尽人意。
其实KMP可以理解为在上述基础上的优化，就是通过已知信息，去省去一些不必要的操作。

## PMT
KMP算法的核心，是一个被称为部分匹配表(Partial Match Table)的数组。也就是next数组。先引入两个概念，前缀，后缀。前缀指的是 存在字符串A和B，存在A=BS，其中S是任意的非空字符串，那就称B为A的前缀。注意，前缀后缀不能为其本身。如图：
![TIM截图20190112130159.jpg](TIM截图20190112130159.jpg)
PMT中值的意义是字符串的前缀集合与后缀集合的交集中最长元素的长度，如图：
![TIM截图20190112130627.jpg](TIM截图20190112130627.jpg)
为了方便读者理解，下面给出字符串"ababa"的所有PMT值。
![TIM截图20190112160007.jpg](TIM截图20190112160007.jpg)
接下来就是解释我们得出的PMT的作用在哪了。我们得出的PMT可以加速字符串的查找。以"ababababca"字符串A来做例子，假设我们需要在A中查找字符串B其值为"abababca"。
![TIM截图20190112163118.jpg](TIM截图20190112163118.jpg)
现在我们比对到第7位的时候，a与c不匹配，暴力的算法是字符串B（下面）的右移一位，继续匹配，但是我们会发现没有意义，因为我们已经知道前6位是匹配的。现在我们算一下字符串B的PMT的值。
![TIM截图20190112164031.jpg](TIM截图20190112164031.jpg)
PMT[7 - 1] = 4。简而言之，在i处适配，那么前面6位是相互匹配的。又因为我们要查找的B字符串的它的4位前缀与4位后缀是相同的 。所以我们可以推导出字符串A之前的4位字符与字符串B开头的4位字符串相同，我们不应该右移一位，而是4位。
![TIM截图20190112164659.jpg](TIM截图20190112164659.jpg)
注意：i坐标不变，变的是j坐标。
通过上面的例子，我们可以使用PMT数组来加快查找，你可以发现，其实右移两位，也可以得到平衡的状态，但是我们选择四位，因为那是交集的最长元素的长度，读者可以自己算下交集，你会发现交集也有长度为2的元素。所以我们取最长的元素，是最优的。综上所述，我们根据前缀和后缀相同的性质，来进行“跨步”，就是我可以把步子迈大，这样速度就快了。

## 如何求next数组。
你会发现我们在上个例子里，第七位的时候，我们选了第六位的PTM值，所以为了变成方便，我们往往会整体向右移一位，然后把第0位填-1，因为这个没有任何意义。所以我们可以在例子的PMT数组上得到next数组。
![TIM截图20190112183040.jpg](TIM截图20190112183040.jpg)
其实next数组用手推是很快的，用代码的话，假如暴力去解，还是没有意义，因为你会发现，去求next数组就花去了大量的时间。其实next数组的求解，就是去求自己！我求我自己？其实就是递归。根据当前得出的next数组，来推出下一个next值。
next数组的过程完全可以看成字符串的匹配过程，即以模式字符串为主字符串，以模式字符串的前缀为目标字符串。（原理和例子相同，多多体会，关键在于模式串的前缀 要与 当前i坐标之前的n位的后缀 相同！！！然后进行右移）

## next代码
```C
int next[100];
//传入模式串 
void gen_next(string s){
	next[0]=0;
	next[1]=0;
	int k=0; 
	for(int i=1;i<s.size();i++){
		while(k>0 && s[k]!=s[i]) k = next[k];
		if(s[k]==s[i]) k++;
		next[i+1]=k; 
	}
	next[0]=-1;
}
```

## KMP代码
宗旨是i指针不动，j选择性回退。所以在匹配过程中会出现两种情况。
1. 匹配。i，j指针都++
2. 不匹配。i不动，j指针回退，回退个数由next数组决定（模式字符串的前缀 与 当前i指针的前n个字符串（可以理解为后缀）匹配）

```C
int kmp(string s, string p)
{
	int lens=s.size(); //主字符串 
	int lenp=p.size(); //模式字符串 
	int i=0,j=0;
	while(i<lens && j<lenp){
		//因为在得到next数组的时候，最后把next[0] = -1
		//所以j==-1代表当前无意义，重新匹配 
		if( j==-1 || s[i]==p[j]){
			i++;
			j++;
		}else j=next[j];
	}
	if(j==lenp) return i-j;
	else return -1;
}
```
## 总代码
### 得到首个匹配下标
```C
#include<bits/stdc++.h>
using namespace std;
int next[100];
//传入模式串 
void gen_next(string s){
	next[0]=0;
	next[1]=0;
	int k=0; 
	for(int i=1;i<s.size();i++){
		while(k>0 && s[k]!=s[i]) k = next[k];
		if(s[k]==s[i]) k++;
		next[i+1]=k; 
	}
	next[0]=-1;
}

int kmp(string s, string p)
{
	int lens=s.size(); //主字符串 
	int lenp=p.size(); //模式字符串 
	int i=0,j=0;
	while(i<lens && j<lenp){
		//因为在得到next数组的时候，最后把next[0] = -1
		//所以j==-1代表当前无意义，重新匹配 
		if( j==-1 || s[i]==p[j]){
			i++;
			j++;
		}else j=next[j];
	}
	if(j==lenp) return i-j;
	else return -1;
}

int main()
{
	string s,p;
	   // 主字符串s ，模式串p  
	cin >> s >> p;
	gen_next(p);
	cout << kmp(s,p) << endl;
} 
```
### 得到所有匹配下标
```C
#include<bits/stdc++.h>
using namespace std;
int next[100];
int ans[1005];
int index=0;
//传入模式串 
void gen_next(string s){
	next[0]=0;
	next[1]=0;
	int k=0; 
	for(int i=1;i<s.size();i++){
		while(k>0 && s[k]!=s[i]) k = next[k];
		if(s[k]==s[i]) k++;
		next[i+1]=k; 
	}
	next[0]=-1;
}

void kmp(string s, string p)
{
	int lens=s.size(); //主字符串 
	int lenp=p.size(); //模式字符串 
	int i=0,j=0;
	while(i<lens && j<lenp){
		//因为在得到next数组的时候，最后把next[0] = -1
		//所以j==-1代表当前无意义，重新匹配 
		if( j==-1 || s[i]==p[j]){
			i++;
			j++;
		}else j=next[j];
		if(j==lenp){
			ans[index++]=i-j;
			//回退
			j=next[j];
		} 
	}
}

int main()
{
	string s,p;
	   // 主字符串s ，模式串p  
	cin >> s >> p;
	gen_next(p);
	kmp(s,p);
	if(index==0){
		cout << "没有匹配到" << endl; 
	}else{
		for(int i=0;i<index;i++){
			cout << ans[i] << " ";
		}
	}
} 
```
