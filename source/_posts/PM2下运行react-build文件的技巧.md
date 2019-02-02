---
title: PM2下运行react build文件的技巧
date: 2019-01-22 14:49:53
tags:
- pm2
- react
- linux
---

## 环境：
腾讯云学生特价服务器 linux ubuntu系统

## 方法：
1. 安装pm2, 上传build文件
2. 在build目录下新建start.sh文件，其内容为：
```
serve -l [portNum]
```
3. 在build目录下运行命令：
```
pm2 start start.sh
```
这样就可以比较方便的使用pm2来部署react的build项目了~
