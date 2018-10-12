---
title: Redis学习(2)Windows下安装和可视化工具安装
date: 2018-09-11 21:31:10
categories:
- Redis
---
## 1.安装 redis
[github下载地址](https://github.com/MSOpenTech/redis/releases)
进去下载对应的文件就好，但是我下载的很慢，不知道为啥，可能是对方服务器的问题，所以我就去阿里云服务器上 wget下载了，速度还过得去，然后拉到本地。
接下来的安装和别的没差，记住勾选 添加到本地环境就好。
2.redis使用
cmd下面运行
```Bash
redis-server.exe redis.windows.conf
```
假如没报错，说明就OK了。
报错的话可能是（因为我就遇到了这个）
```Bash
# Creating Server TCP listening socket 127.0.0.1:6379: bind: No error
```
解决方法
```Bash
redis-cli.exe
shutdown
exit
```
然后再运行启动的命令就好了，看到redis的logo就说明启动了
3.连接
接下来打开另一个窗口
cmd运行
```Bash
redis-cli.exe
```
4.修改
下面就可以修改我们redis里面的东西了
```Bash
set username abc
```
shell 会打印 ok
说明修改成功

## 2.安装Redis Desktop Manager
[网盘地址](https://pan.baidu.com/s/1kU8sY3P)

下载安装就行
安装好后，左下角有个 connect
name:随意
host:127.0.0.1
port:默认
这样就配置好了。

