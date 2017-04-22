---
title: python 笔记 1 #文章標題
date: 2016-11-02 #文章生成時間
categories: "python" #文章分類目錄 可以省略
tags: python 入门 笔记 #文章標籤 可以省略
description: #你對本頁的描述 可以省略
---
表是以类的形式实现的。“创建”列表实际上是将一个类实例化。因此，列表有多种方法可以操作。

1. 列表可包含任何数据类型的元素，单个列表中的元素无须全为同一类型。
2.  append() 方法向列表的尾部添加一个新的元素。只接受一个参数。

3.  extend()方法只接受一个列表作为参数，并将该参数的每个元素都添加到原有的列表中。

4.  python join 连接字符串 ”.join(…) or join(”,…)

5. Python split()方法  描述

Python split()通过指定分隔符对字符串进行切片，如果参数num 有指定值，则仅分隔 num 个子字符串

语法

split()方法语法：

str.split(str="", num=string.count(str)).

参数
str — 分隔符，默认为空格。num — 分割次数。

返回值

返回分割后的字符串列表。

实例

以下实例展示了split()函数的使用方法：

#!/usr/bin/python

str = "Line1-abcdef \nLine2-abc \nLine4-abcd";
print str.split( );
print str.split(' ', 1 );
以上实例输出结果如下：

['Line1-abcdef', 'Line2-abc', 'Line4-abcd']
['Line1-abcdef', '\nLine2-abc \nLine4-abcd']
