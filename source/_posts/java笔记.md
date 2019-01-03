---
title: java笔记
date: 2019-01-03 10:35:44
tags:
---

# Java题目复习笔记

## Java虚拟机

- Java文件经过javac编译变成字节码文件(后缀是.class)，字节码文件是统一的，经过不同平台上的JVM编译成CPU可以理解的机器码。

## Java文件规范

- Java的源文件格式必须是.java后缀
- 一个Java编译命令可能产生几个字节码文件
- 运行字节码文件需要调用java命令
- Java的入口main函数必须是public static void前缀且接收参数为string []
- 一个Java源文件可以有多个main但只能有一个符合入口名函数标准的main.
- Java是半编译半解释型语言：扯tm的蛋。
- java.lang是自动导入的
- 