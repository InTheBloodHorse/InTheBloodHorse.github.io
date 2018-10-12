---
title: Flask 笔记
date: 2018-09-26 22:04:12
categories:
- Flask
---
### 随机查询模型12条数据
```Python
from sqlalchemy.sql.expression import func
photos = Photo.query.order_by(func.rand()).limit(12)
```
python内置的random模块提供的choice()方法进行随机排序的性能远不及数据库引擎提供的随机函数。
### 查询使用次数最多的前十个标签
```Python
from sqlalchemy.sql.expression import func
tags = Tag.query.join(Tag.photos).group_by(Tag.id).order_by(func.count(Photo.id).desc()).limit(10)
```
Tag.query:从Tag模型开始查询
join(Tag.photos):联结Tag和Photo
group_by(Tag.id):根据Tag记录的id分组
order_by(func.count(Photo.id).desc()):使用通用函数count()统计各组记录的数量，然后降序排序
limit(10):限制10条
