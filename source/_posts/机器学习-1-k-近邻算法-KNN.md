---
title: 机器学习(1) k-近邻算法(KNN)
date: 2018-09-15 15:10:24
categories:
- 机器学习
---
简单地说，k-近邻算法采用测量不同特征值之间的距离方法进来分类 
## 特点：

### 优点：
精度高、对异常值不敏感、无数据输入假定
### 缺点：
计算复杂度高、空间复杂度高
### 适用数据范围：
数值型和标称型

k-近邻算法称为kNN，它的工作原理是：存在一个样本数据集合，也称作训练样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每一数据与所属分类的对应关系。输入没有标签的新数据后，将新数据的每个特征与样本集中数据对应的特征进行比较，然后算法提取样本集中特征最相似数据（最近邻）的分类标签。一般来说，我们只选择样本数据集中前κ个最相似的数据，这就是k-近邻算法中κ的出处。通常κ是不大于20的整数。最后，选择κ个最相似数据出现次数最多的分类，作为新数据的分类。

总结：就是输入数据，与样本数据之间进行计算（差值，比如两点之差），然后进行排序，拿到前K个距离最小的，输出前K个出现标签最多的。

## 代码流程
1.计算已知类别数据集中的每个点依次执行以下操作
2.计算已知类别数据集中的点与当前点之间的距离
3.按照距离递增次序排序
4.选择与当前点距离最小的κ个点
5.确定前κ个点所在类别的出现概率
6.返回前κ个点出现频率最高的类别作为当前点的预测分类

Python代码
```Python
import numpy as np


# 创造数据
def create_data_set():
    group = np.array([
        [1.0, 1.1],
        [1.0, 1.0],
        [0, 0],
        [0, 0.1]
    ])
    labels = ['A', 'A', 'B', 'B']
    return group, labels


def classify0(input_data, data_set, labels, k):
    data_size = data_set.shape[0]
    # 转换成与训练数据相同的shape
    diff_mat = np.tile(input_data, (data_size, 1))
    dis = diff_mat - data_set
    # 平方
    sqrt_dis = dis ** 2
    # 求和
    sum_row_dis = sqrt_dis.sum(axis=1)
    # 开方
    dis = sum_row_dis ** 0.5

    index = dis.argsort()
    cnt = {}
    for i in range(k):
        label = labels[index[i]]
        cnt[label] = cnt.get(label, 0) + 1
    # 返回频率最大的值的key
    return sorted(cnt.items(), key=lambda x: x[1], reverse=True)[0][0]


data_set, labels = create_data_set()
print("分类为:" + classify0([0, 0], data_set, labels, 3))

```
