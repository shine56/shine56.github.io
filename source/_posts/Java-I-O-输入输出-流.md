---
title: Java--I/O(输入输出)流
date:  2019-01-28 21:33:49
edited:
tags: Java入门
categories: Java学习
---
# 流？I/O？
>流是一组有序的数据序列，根据操作可分为输入流和输出流即（I/O流），I/O流也可以理解为一种通道程序。而I/O包提供给了很多工具(类)对I/O流进行操作，从而达到安全地将**源数据通过流传送到目的地**的目的。源和目的地可以是磁盘，键盘，鼠标，显示器，网络，压缩包等等。

<!--more-->
# 输入/输出流
## 将数据从流里读取进来——输入流

* I/O包给我们提供给了两个父类进行数据从流读取进来的操作。
* 其一**抽象类InputStream**(字节输出流)。该类是所有字节输入流的父类，遇到错误会引发IOException异常。
* 其二**抽象类Reader**(字符输出流)，该类适用于处理字符文本，是所有字符输入流的父类。

## 将数据输出到流里里边——输出流
* I/O包给我们提供给了两个父类进行数据输出到流的操作。
* 其一**OutputStream**(字节输出流)。该类是所有字节输出流的父类，该类所有方法皆返回 **void**，遇到错误会引发IOException异常。
* 其二 **Writer**(字符输出流)，该类适用于处理字符文本，是所有字符输出流的父类。

## I/O流常用子类
**看到这便知，输入流与输出流的操作都各有两个类可以操作。另外**Reader**和**Writer**类有两个子类可以对流中的 字节/字符 进行转换输入输出。**

* InputStreamReader：将流中的字节转换成字符读取进来。
* OutputStreamWriter: 将字符转换成字节输出到流。
* 这两个类通常用作读/写磁盘文件。

**上述的输入/输出流当中有一种子类：带缓存的输入/输出流。缓存是I/O的一种性能优化。缓存流为I/O增加了内存缓存区。**

四个子类(缓存流)如下：
* BufferedInputStream
* BufferedOutputStream
* BufferedReader
* BufferedWriter

# File类
**该类主要用于文件和目录的创建、文件的查找和文件的删除等。File对象代表磁盘中实际存在的文件和目录。可以通过以下构造方法创建一个File对象。**

## 文件创建
>new File(String pathname) 如：File abc = new ("d:/1.txt");

>new File(String parent,String child), String parent是父路径字符串， String child是子路径字符串。

>new File(File f,String child); File f父路径对象，String child是子路径字符串。

## 获取文件信息
* File提供了许多方法获取文件本身信息。

方法|返回值|说明
:---|:---|:---
getNamw()|String|获取文件名称
canRead()|boolean|判断文件是否可读
canWrite()|boolean|判断文件是否可写
exits()|boolean|判断文件是否存在
length()|long|获取文件长度(字节为单位)
getAbsolutePath()|String|获取文件绝对路径
getParent()|String|获取文件父路径
lastModified()|long|获取文件最后修改时间
delete()|void|删除文件
