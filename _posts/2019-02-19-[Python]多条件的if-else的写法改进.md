---
layout: post
title:  "[Python]多条件的if-else的写法改进"
date: 2019-02-19 10:05:29 +0800
categories: [coding]
---


# 背景
在工作中，看到很多小哥哥和小姐姐写多条件判断的时候，用了N多的if，else
代码可读性不高，而且维护成本高

# Bad写法
比如：判断一个人的分数等级A:输出优秀，B：输出良好，C：输出合格，D:输出不合格
``` python
if a == 'A':
  print('优秀') 
elif a == 'B':
  print('良好')
elif a == 'C':
  print('合格')
elif a == 'D':
  print('不合格')
else:
  print('未定义')
```
这样写是不是每次增加一个条件都要扩展一个elif？

# 改良写法
``` python
cond = [('A','优秀',print),
        ('B','良好',print),
        ('C','合格',print),
        ('D','不合格',print),
      ]
val = '未定义'
func = print
for (x,y,f) in cond:
  if a == x:
    val = y
    func = f
f(val)
```
这样改良后，我只需要维护cond就可以任意的扩展和维护了。


