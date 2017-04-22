---
title: linux 文件编码 #文章標題
date: 2016-11-07 #文章生成時間
categories: "linux" #文章分類目錄 可以省略
tags: linux 入门 编码 #文章標籤 可以省略
description: #你對本頁的描述 可以省略
---
ASCII 到 UTF-8 文件编码格式 转换
linux 默认编码格式是 ASCII 码，在显示或者保存中文汉字的时候会出现乱码或显示为点，解决办法是 配置系统语言为 UTF-8
首先查看系统语言
```
echo $LANG
```
显示不是 中文编码，查看系统语言包
```
locale
```
如果没有 utf-8 编码，那么需要安装支持
```
yum groupinstall chinese-support
```
设置显示语言为中文，并查看设置
```
LANG="en_US.UTF-8"

echo $LANG
```

结果我的还是无法正确显示，我猜测问题是 putty 的设置问题，更改设置中的 translation 的 remote character set 为 utf-8，完美解决中文显示支持问题。
至于文件编码的转换，有两种方式。
首先查看文件编码格式:   
```
file -i example
```
查看file 支持的文件编码格式
```
file -l
```
法一：使用
 ```
iconv -f encoding1 -t encoding2  example1 -o example2
```
进行转换
其中 encoding1 是 examplel1 的编码格式      encoding2  是 example2 的编码格式
法二： vim 中转换 :
```
set fileencoding=utf-8
```
