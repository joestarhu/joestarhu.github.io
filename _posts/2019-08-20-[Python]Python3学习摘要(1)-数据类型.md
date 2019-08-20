---
layout: post
title:  "[Python]Python3学习摘要(1)-数据类型"
date: 2019-08-20 11:59:49 +0800
categories: [coding]
---

# Python简单介绍
## Python的版本
Python有2个版本：
- Python2.x
- Python3.x

在运行python interpreter（Python解释器）的时候,输入python和python3使用的是不同的版本
* 一般python指代python2.x版本，
* python3指代python3.x版本

```
$ python
Python 2.7.10 (default, Feb 22 2019, 21:55:15)  
[GCC 4.2.1 Compatible Apple LLVM 10.0.1 (clang-1001.0.37.14)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>

$ python3
Python 3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 16:39:00)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

>建议使用python3，因为一些库已经准备开始舍弃对python2的支持了。所以后续我都是以python3为基础说明

## Python的文件和编码
* Python的文件以.py结尾
* Python的内容是大小写敏感的
* python3的默认文件编码格式为utf8

通过在文件的第一行使用如下代码，可以设定文件编码
```python
# -*- coding:utf-8 -*-
```
当使用了shebang的时候，这个声明应该放在第2行
```python
#! /usr/bin/env python3
# -*- coding:utf-8 -*-
```
由于python3默认的文件编码格式为utf-8，一般我们不需要再去设定这个编码了

# 数据类型
## 整数类型-int
``` python
# 查看数据类型可以用type函数
type(1) # 输出int

# 整数类型-int
-1,1,0,999 # 10进制的整数类型，通常我们用的最多的
-0b11   #0b打头表示2进制，符号可以加在0b前面
0o7     #0o打头表示8进制
0xa     #0x打头表示16进制
```
## 浮点数类型-float
```python
# 带小数点的被成为浮点数类型-float
# 下面是几种浮点数的表示方法
1.   #等同于1.0
1.0
-1.1
1.1e-1 # 科学计数法 等于0.11
1.1e1  # 科学计数法 等于11.0
```

## 复数类型-complex
```python
1+1j #+前面的代表实部，j前面的数字代表虚部
```

## 布尔类型-bool
```python
True  # 非零值 ※任何非零值都被认为True，但是True转换成整数就是1
False # 零值
```

## 字符串类型-str
```python
# 字符串类型，和其他语言不一样，python没有字符类型
'123' # 可以用单引号表示
"123" # 可以用双引号表示

'this\'s a quote sample'  #通过转义字符单引号里表示单引号
"this's a sample"         #不通过转义字符

# 字符串前加上u，代表用unicode编码，比如有中文，日文，韩文等字符的时候，用这个可以防止乱码
u'我是中文呀'

# 字符串前加上r，代表不转义字符,当使用正则表达式的时候配合使用这个
r'this is\" another sample '

# 字符串前面加上b，代表用bytes对象，主要用于网络数据传送的时候
b'<h1>This is a respnse</h1>'
```

## 列表类型-list
```python
#列表类型用[]来表示，列表里面的数据类型可以不一致
#可以通过下标来获取指定值，默认从0开始
[1,2,3,4]           # list内部元素的类型一致  
[1,1.1,True,'STR']  # list内部元素的类型不一致
[]                  # 空的list
```

## 元祖类型-tuple
```python
#元祖类型用()来表示，元祖里面的数组类型可以不一致
#可以通过下标来获取指定值，默认从0开始
(1,2,3,4)           # tuple内部元素的类型一致  
(1,1.1,True,'STR')  # tuple内部元素的类型不一致
()                  # 空的tuple
```

## 字典类型-dictonary
```python
#字典类型用{}来表示，字典里面的数组类型可以不一致
#字典的存储形式是Key-Value形式，Key是
{}                #空的字典
{'name':'Major','age':27,3:123} #3个Key-Value
```

## 集合-set
```python
#集合也是用{}来表示，集合里面的数值不会重复
#集合不能用下标来获取指定值
set()           # 空的集合必须用set函数
set([1,2,2,1])  # 集合的内容{1,2}
```
