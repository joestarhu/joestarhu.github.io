---
layout: post
title:  "[Python]Python3学习摘要(001)-Python基础"
date: 2020-02-03 13:17:52 +0800
categories: [Python学习]
---

# Python简介
## 创造者
Python是由**Guido van Rossum**是为了打发无聊的圣诞节设计的

## 官方网站
https://www.python.org/

## 版本
- Python分为2.x和3.x 2个版本(x代表2位版本号,比如:2.1, 2.2, 3.1, 3.2)
- 虽同为Python语言,但2个版本的兼容性并不好
- 3.x将会是未来的趋势.所以建议使用**3.x**

## 文件后缀
Python的文件后缀名都为.py文件

## 代码大小写敏感
python大小写敏感,即a和A是不一样的

## 代码注释
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

## 文件编码
Python3的文件编码默认为utf-8,一般不需要去特别设置,但是仍可以通过如下代码设置成你想要的编码,文件编码需要放在开头(如有shebang,则放在shebang后面)
``` python
# -*- encoding:utf-8 -*-

print('hello Python')
```

``` python
#! /usr/bin/env python3
# -*- encoding:utf-8 -*-

print('hello Python')
```
**※如无特殊必要,建议不要去设置,默认utf-8基本满足所有条件**

## Python程序如何运行
Python是通过Python解释器来解释python代码并执行,因此在运行Python程序之前,你需要有一个python解释器

# Python环境整备
## Python编辑器
有很多编辑器供大家选择,选择自己喜欢的一款即可.常见的有:
* Pycharm
* Vs Code
* Sublime
* Atom

## Python解释器
从 https://www.python.org/downloads/ 这里下载你所需要的解释器并安装

## 虚拟环境
python因为有2个版本,兼容性以及为保持环境的干净,一般都会使用虚拟环境
虚拟环境的创建如下:
```
>先创建一个虚拟文件夹,命令如下
$ mkdir pyenv
$ cd pyenv
>然后创建虚拟环境
$ virtualenv venv

>创建完成后,文件家目录如下:
|-pyenv
  |-venv

>运行虚拟环境
$ source venv/bin/activate
运行成功后,命令行前面会出现(venv)的标志

>然后你可以在虚拟环境中,安装你所需要的各种包(pip命令)

>退出虚拟环境
(venv)$ deactivate
运行成功后,命令行前面(venv)的标志消失
```

## 运行Python解释器

|命令|说明|
|:--|:--|
|python2|运行python2的解释器|
|python3|运行python3的解释器|
|python|根据你的环境变量,执行2或3的解释器|

## 通过解释器运行Python程序

|版本|命令行运行命令|
|:--|:--|
|2.x|python2 a.py|
|3.x|python3 a.py|

## 通过shebang运行python程序
Python可以当作脚本文件执行,只需要在文件的第一行编写shebang语句即可,一般默认为

|版本|shebang写法|命令行运行命令|
|:--|:--|:--|
|2.x|#! /usr/bin/env python2|./a.py|
|3.x|#! /usr/bin/env python3|./a.py|

※如果文件无法执行,可能是文件的权限配置不正确,执行命令 **chmod 777 ./a.py** 即可

