---
title: python 笔记 2 #文章標題
layout: default
date: 2016-11-03 14:35:48 #文章生成時間
categories: "python" #文章分類目錄 可以省略
tags: python 入门 笔记 #文章標籤 可以省略
description: #你對本頁的描述 可以省略
---
?r File “ex8a.py”, line 2 x = %r %r % r %r ^ SyntaxError: invalid syntax print x 标签（空格分隔）： python学习

—笔记
###1.# — coding：utf=8 — 忘记了这句，这句一定要在中文注释前面打上，不然会报错

2.打印字符串“I had this thing.”,“That you can type up right.”,“But it didn’t sing.”, “So I said goodnight.”逗号在这里不打印出来，起一个假想的连接作用。print 固定格式是 “print 加 变量名 加 % 加 （括号里面有内容）”

3.print formatter % (True, False, False, True) #在屏幕上打印出四个单词，为什么不用括起来打印

4.formatter = “%r %r %r %r” #将字符串赋值给formatter File “ex8.py”, line 3 formatter = 鈥?r %r %r %r鈥? #灏嗗瓧绗︿覆璧嬪€肩粰formatter

^

SyntaxError: invalid syntax 当你把错误修正好了，再去记下经验教训 “”””这是不一样的

5.print 这是啥 print print print print print print print print print

6.print formatter % (formatter,formatter,formatter,formatter)#看一看结果是啥，我猜是四个单词，或者变量名，“formatter*4”你这次猜错了，是‘%r %r %r %r’ ‘%r %r %r %r’ ‘%r %r %r %r’ ‘%r %r %r %r’

7.print “”

8.x =”%r %r % r %r” x = %r %r % r %% (1,2,3,4) print x % (“o,o,o,o”) print x % (“mmmmmmmmmm mmmmmmmm”,”mmjjjjjj,mmmm.”,”lllll.”,”sskdke”) print x % (x,x,x,x) x = “%r %r % r %r” print x % (1,2,3,4) 1 2 3 4 print x % (“one”,“fff”,“fffo,fffo”,“52d”) ‘one’ ‘fff’ ‘fffo,fffo’ ‘52d’ print x % (“mmmmmmmmmm mmmmmmmm”,“mmjjjjjj,mmmm.”,“lllll.”,“sskdke”) ‘mmmmmmmmmm mmmmmmmm’ ‘mmjjjjjj,mmmm.’ ‘lllll.’ ‘sskdke’ print x % (x,x,x,x) ‘%r %r % r %r’ ‘%r %r % r %r’ ‘%r %r % r %r’ ‘%r %r % r %r’ print x % (‘x’,‘x’,‘x’,‘x’) ‘x’ ‘x’ ‘x’ ‘x’ 没加引号，直接输出，加了单双引号输出单引号加字符串，字符串之间用“，”隔开，注意中英文编码的区别。

9.python单引号、双引号和三双引号的区别 python字符串通常有单引号（‘…’）、双引号（“…”）、三引号（“”“…”“”）或（‘’‘…’‘’）包围，三引号包含的字符串可由多行组成，一般可表示大段的叙述性字符串。在使用时基本没有差别，但双引号和三引号（“”“…”“”）中可以包含单引号，三引号(‘’‘…’‘’)可以包含双引号，而不需要转义

如： s1 = “hello,world” 如果要写成多行，那么就要使用 (“连行符”)吧， 如：s2 = “hello,  world”

s2与s1是一样的。如果你用3个双引号的话，就可以直接写了， 如：s3 = “”“hello, world,

hahaha.”“”， 那么s3实际上就是“hello,\nworld,\nhahaha.”, 注意“\n”，所以，如果你的字符串里\n很多，你又不想在字符串中用\n的话，那么就可以使用3个双引号。而且使用3个双引号还可以在字符串中增加注释， 如：s3 = “”“hello, #hoho, this is hello, 在3个双引号的字符串内可以有注释哦 world, #hoho, this is world hahaha.”“”

这就是3个双引号和1个双引号表示字符串的区别了，3个双引号与1个单引号的区别也是和这个一样的， 当字符串需要加入引号时，可采用单引号与双引号互相嵌套使用 例如：print ‘test “’“test ”‘”’ –>> test “test” “test ‘”‘test “’“ –>> test ‘test’ 实际上python支持单引号是有原因的，下面我来比较1个单引号和1个双引号的区别。当我用单引号来表示一个字符串时，如果要表示 Let’s go 这个字符串，必须这样： s4 = ‘Let’s go’，注意没有，字符串中有一个‘，而字符串又是用’来表示，所以这个时候就要使用转义符  （，转义符应该知道吧）, 如果你的字符串中有一大堆的转义符，看起来肯定不舒服，python也很好的解决了这个问题， 如：s5 = ”Let’s go“ 这时，我们看，python知道你是用 ” 来表示字符串，所以python就把字符串中的那个单引号 ‘ , 当成普通的字符处理了，是不是很简单。对于双引号，也是一样的，下面举个例子 s6 = ‘I realy like “python”!’这就是单引号和双引号都可以表示字符串的原因了。

10.%r调用rper函数打印字符串,repr函数返回的字符串是加上了转义序列,是直接书写的字符串的形式%s调用str函数打印字符串,str函数返回原始字符串

11.‘单引号输出会转义 ’ 当然是单双引号都有的情况下

12.python -m pydoc raw_input

13.习题十二讲的是如何在raw_input(…)里加上注释提示别人

14.promapt 错

是prompt

1st 是无效的SyntaxError: invalid syntax

%

15.script 脚本，在from sys import argv

script， filename = argv 里面不用用户赋值，自动定义为当前文件名

15.严重错误

txt_again.read

正确的是 txt_again()

txt = open(filename)

**> 16.如何直接用open read 留个问题**

**>txt_again.close()  关闭文件**
