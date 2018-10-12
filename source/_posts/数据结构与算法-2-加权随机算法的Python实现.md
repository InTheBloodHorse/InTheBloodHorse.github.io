---
title: 数据结构与算法(2) 加权随机算法的Python实现
date: 2018-09-17 17:20:22
categories:
- 数据结构与算法
---
# 随机抽奖算法
## 1.随机抽取一个或者多个
```Python
import random

name = ['a', 'b', 'c', 'd', 'e', 'f']

random_name = random.choice(name)
print(random_name)
random_names = random.sample(name, 3)
print(random_names)

```
结果：
```Python
d
['a', 'b', 'f']
```
## 考虑权重后随机抽取一个元素
如果不同的成员有不用的权重，比如某个成员是会员，某个成员贡献度高等，可以自定义一个抽取函数，用来抽取一个元素
算法的思路就是
### 1. 拿到 0 ~ 权重之和之间的一个数字
这个数字作为判定的标准
### 2. 累加计算
定义初始值0 ，累加计算，假如标准值 小于等于 累加的值，返回结果
### 3.总结，说实话这个很绕，不过可以这样理解，把权值分段
data数据
```Python
data = {
        '老王': 1,
        '阿汤哥': 7,
        '伟哥': 3,
        '阿黄': 3,
        '阿绿': 2,
        '王师傅': 5,
        '狗贼': 1
    }
```
那么我们可以分段为[0,1],(1,8],(8,11],(11,14],(14,16],(16,21],(21,22]
然后看我们random的标准值在哪个区间里面
```Python
import random


def random_weight(data):
    total = sum(data.values())
    st = random.uniform(0, total)
    curr_sum = 0
    for key in data.keys():
        curr_sum += data[key]
        if st <= curr_sum:
            ret = key
            return ret


if __name__ == '__main__':
    data = {
        '老王': 1,
        '阿汤哥': 7,
        '伟哥': 3,
        '阿黄': 3,
        '阿绿': 2,
        '王师傅': 5,
        '狗贼': 1
    }
    print(random_weight(data))
```
大致的概率，和权重几乎符合
```Bash
阿黄 0.1362605
阿汤哥 0.3185684
阿绿 0.0909506
王师傅 0.2270081
伟哥 0.136271
老王 0.04555
狗贼 0.0453914
```
## 选择排序
思路是在先从大到小排序，再去判断，因为我们权值大的话，随机的几率会大，所以可以在某种程度上，加快速度。

