---
title: Redis学习(1) Linux下安装
date: 2018-09-11 19:25:44
categories:
- Redis
---
1.安装
```Bash
sudo apt-get install redis-server
```
2.卸载
```Bash
sudo apt-get pruge --auto-remove redis-server
```
3.启动：安装redis后，默认自动启动，通过以下查看
```Bash
ps -ef|grep redis
```
或者
```Bash
sudo service redis-server start
```
3.停止
```Bash
sudo service redis-server stop
```
