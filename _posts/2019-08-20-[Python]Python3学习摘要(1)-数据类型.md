---
layout: post
title:  "[Python]Python3学习摘要(1)-数据类型"
date: 2019-08-20 11:59:49 +0800
categories: [coding]
---

# 数据类型
## 数字
### 整数-int,bool
``` python
# 在Python中，有2种整数类型：int和bool

# int
-1,1,0,999  # 10进制的整数类型，通常我们用的最多的
-0b11       # 0b打头表示2进制，符号可以加在0b前面
0o7         # 0o打头表示8进制
0xa         # 0x打头表示16进制
100_000_000 # 可以使用下划线分割数字，这点很帅气，大数字的时候可读性大大的增加了

# bool
True  # 非零值 ※任何非零值都被认为True，但是True转换成整数就是1
False # 零值
```

### 浮点数-float
```python
# 带小数点的被成为浮点数类型-float
# 下面是几种浮点数的表示方法
1.        # 等同于1.0
1.0,-1.1  # 常用的写法
1.1e-1    # 科学计数法 等于0.11
1.1e1     # 科学计数法 等于11.0
```

### 复数-complex
```python
1+1j #+前面的代表实部，j前面的数字代表虚部

a = 1+1j
a.real  # .real 获取复数的实部
a.imag  # .imag 获取复数的虚部
```

# 序列
## immutable序列(即值创建后不可更改)
### 字符串类型-str
``` python
# 字符串类型，python没有字符类型,字符串是immutable的
'123' # 可以用单引号表示
"123" # 可以用双引号表示
'this\'s a quote sample'  #通过转义字符单引号里表示单引号
"this's a sample"         #不通过转义字符

# 字符串前加上u，代表用unicode编码，比如有中文，日文，韩文等字符的时候，用这个可以防止乱码
u'我是中文呀'

# 字符串前加上r，代表不转义字符,当使用正则表达式的时候配合使用这个
r'this is\" another sample '
```

### 字节-bytes
``` python
# 字符串前面加上b，代表用bytes对象，主要用于网络数据传送的时候
b'<h1>This is a respnse</h1>'

b'你好' # 会出错,只能是asci编码字符
a = '你好'
b = a.encode('utf-8') # 通过encode函数可以让str -> byte
b.decode('utf-8')     # 通过decode函数可以让byte -> str
```

### 元祖类型-tuple
```python
#元祖类型用()来表示，元祖里面的数组类型可以不一致
(1,2,3,4)           # tuple内部元素的类型一致  
(1,1.1,True,'STR')  # tuple内部元素的类型不一致
()                  # 空的tuple
```

## mutable序列(即值创建后仍可更改)
### 列表类型-list
```python
#列表类型用[]来表示，列表里面的数据类型可以不一致
[1,2,3,4]           # list内部元素的类型一致  
[1,1.1,True,'STR']  # list内部元素的类型不一致
[]                  # 空的list
```

### 字节串数组-byte array
```python
# 通过bytearray函数创建对象



```

## 序列通用操作
```Python
# len函数获取长度
a = '123456'
l = len(a) 

# 可以通过索引访问序列内的对象，索引默认从0开始
a[0]  # '1'
# 索引也可是负数，代表从最后面开始
a[-1] # '6'

# 序列可以使用切片 a[i:j:k] 
a[1:6:2]
```

# 集合
## Set(Mutable)
```python
# 集合也是用{}来表示，集合里面的数值不会重复
# 集合不能用下标来获取指定值,Set是mutable的
set()           # 空的集合必须用set函数
set([1,2,2,1])  # 集合的内容{1,2}
```
## Frozen Set(Immutable)
```python
# 集合也是用{}来表示，集合里面的数值不会重复
# 集合不能用下标来获取指定值，Frozenset是immutable的
forzenset()           # 空的集合必须用set函数
forzenset([1,2,2,1])  # 集合的内容{1,2}
```

# 映射
## 字典类型-dictonary
```python
#字典类型用{}来表示，字典里面的数组类型可以不一致
#字典的存储形式是Key-Value形式，Key是
{}                #空的字典
{'name':'Major','age':27,3:123} #3个Key-Value
```
