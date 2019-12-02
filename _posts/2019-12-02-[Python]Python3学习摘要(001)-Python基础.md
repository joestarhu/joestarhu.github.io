---
layout: post
title:  "[Python]Python3学习摘要(001)-Python基础"
date: 2019-12-02 19:48:01 +0800
categories: [coding]
---
# Python基础
> Python分为2.X和3.X 2个版本,虽同为Python语言,但2个版本的兼容性并不好,3.X将会是未来的趋势.所以建议使用**3.X**

## Shebang
Python可以当作脚本文件执行,只需要在文件的第一行编写shebang语句即可,一般默认为

``` python
#! /usr/bin/env python3
```
※如果文件无法执行,可能是文件的权限配置不正确,执行命令 **chmod 777 ./[你的python文件].py** 即可

## 文件编码
Python3的文件编码默认为utf-8,一般不需要去特别设置,但是仍可以通过如下代码设置成你想要的编码,文件编码需要放在开头

``` python
#! /usr/bin/env python3
# -*- encoding:utf-8 -*-
```
※建议不要去设置,默认utf-8基本满足所有条件

## 注释
Python注释有3种方法
``` python
# 单行注释用#

'''
可以用3个'注释多行
也可用3个"注释多行
'''

"""
可以用3个'注释多行
也可用3个"注释多行
"""
```
