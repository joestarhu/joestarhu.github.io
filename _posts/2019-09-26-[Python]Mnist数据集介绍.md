---
layout: post
title:  "[Python]Mnist数据集介绍"
date: 2019-09-26 10:12:49 +0800
categories: [coding]
---

# Mnist数据集简介
Mnist数据集来自美国国家标准与技术研究所。数据集的内容是包含了很多人的手写数字图片。  
它包含了60000张训练图片，10000张测试图片。


## 下载地址
Mnist的下载地址：http://yann.lecun.com/exdb/mnist/

|FileName|Description|
|:--|:--|
|train-images-idx3-ubyte|训练集图片,60000张图片|
|train-labels-idx1-ubyte|训练集标签,60000个标签|
|t10k-images-idx3-ubyte|测试集图片,10000张图片|
|t10k-labels-idx1-ubyte|测试集标签,10000个标签|

`下载可以直接下载，也可以通过代码实现`

## 数据集文件的组成

这4个文件的构成如下图：可以看到
图片的数据集，前16位分别表示 魔法数字，图片个数，行数，列数。从17位开始才是正式的图片数据

而标签的数据集，前8位分别表示 魔法数字，标签数字，从第9位开始才是正式的标签数据。

`因此，在读取mnist文件的时候，要把图片的开始位置从第17位开始，标签从第9位开始`

![文件的结构](https://github.com/cantahu/cantahu.github.io/blob/master/pic/Mnist%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84.png?raw=true)

# 使用Mnist的数据集
``` python
#! /usr/bin/env python3

import os             # 用于创建文件夹
import os.path        # 用于判断路径是否存在
import urllib.request # 用于下载文件
import gzip           # 用于打开下载的gzip文件
import pickle         # 用于创建和读取pickle文件
import numpy as np    # 用于numpy的数据转换
import matplotlib.pyplot as plt # 用于读取数据，并展示图片

# mnist的下载地址
mnist_url = 'http://yann.lecun.com/exdb/mnist/'

# mnist的下载文件,用dict形式来存储。后续处理文件会比较方便点
mnist_file = {
  'tr-img':'train-images-idx3-ubyte.gz',
  'tr-lbl':'train-labels-idx1-ubyte.gz',
  'te-img':'t10k-images-idx3-ubyte.gz',
  'te-lbl':'t10k-labels-idx1-ubyte.gz',
}

# 当前文件的文件夹地址
py_dir = os.path.dirname(os.path.abspath(__file__))
# dataset的文件夹地址,把mnist数据都放入dataset文件夹中
ds_dir = py_dir + '/dataset'
# pkl文件的地址
pkl_file = py_dir + '/' + 'mnist.pkl'


class JhuMnist:
  def __init__(self):
      if not os.path.exists(pkl_file):
          self._mnist_download()  # 下载数据集
          self._convert_numpy()   # 转换数据结
          self._save_as_pkl()     # 保存pkl

  def _mnist_download(self):
      """下载mnist数据集"""
      # 判断文件夹是否存在
      if not os.path.exists(ds_dir):
          os.makedirs(ds_dir)
      
      for f in mnist_file.values():
          f_path = ds_dir + '/' + f
          if os.path.exists(f_path):
              continue
          print(f'开始下载{f} ...')
          urllib.request.urlretrieve(mnist_url + f, f_path)
      print('Done')

  def _read_mnist(self,type,f_path) -> np.ndarray:
      # 图片数据集开始的位置和标签数据集开始的位置不一样，见上文的图片描述
      offset = 8 if type == 'lbl' else 16
      
      with gzip.open(f_path, 'rb') as f:
          f = np.frombuffer(f.read(), np.uint8, offset=offset)
      if type == 'img':
          f = f.reshape(-1,28*28)
      return f

  def _convert_numpy(self):
      """转换数据集变成Numpy类型"""
      self.ds = {}
      for i in mnist_file:
          type = 'img' if i.find('img') != -1 else 'lbl'
          self.ds[i] = self._read_mnist(type,ds_dir+'/'+mnist_file[i])

  def _save_as_pkl(self):
      with open(pkl_file, 'wb') as f:
          pickle.dump(self.ds, f, -1)
      print('Done')

  def load_mnist(self) -> dict:
      with open(pkl_file,'rb') as f:
          ds = pickle.load(f)
      return ds

if __name__ == '__main__':
  j = JhuMnist()
  ds = j.load_mnist()
  print(ds['tr-lbl'][0])
  img = ds['tr-img'][0].reshape(28, 28)
  fig =plt.figure()
  plt.axis('off')
  plt.imshow(img, cmap='gray')
  plt.show()
  
```
