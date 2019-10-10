---
layout: post
title:  "[Python]Mnist数据集介绍"
date: 2019-09-26 10:12:49 +0800
categories: [coding]
---

# Mnist数据集介绍
这是一个包含了手写数字的图片集合。官网网站：http://yann.lecun.com/exdb/mnist/

它分成了4个文件：

|FileName|Description|
|:--|:--|
|train-images-idx3-ubyte|训练集图片,60000张图片|
|train-labels-idx1-ubyte|训练集标签,60000个标签|
|t10k-images-idx3-ubyte|测试集图片,10000张图片|
|t10k-labels-idx1-ubyte|测试集标签,10000个标签|


# Mnist的文件结构
![文件的结构](https://github.com/cantahu/cantahu.github.io/blob/master/pic/Mnist%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84.png?raw=true)

1. 所有文件的0x0000～0x0003 表示一个魔数※关于魔数就不展开说明了，反正用不到，你就当一个标签好了
2. 0x0004~0x0007 表示文件的内容大小(比如多少个标签，多少个图片)
3. 标签文件从0x0008开始就是正式的标签数字了，每个byte放一个标签值,标签值范围为0～9
4. 图像文件0x0008～0x0011 代表行数，0x0012～0x0015代表列数,从0x0016开始是每个图片的像素值了。像素值为0~255, 0表示黑色,255表示白色
5. 图片行列代表图片由rows*cols个像素组成.mnist的图片是28行*28列的像素构成的
6. 所以对我们来说，标签真正的内容是从0x0008开始的，图片真正的内容是从0x0016开始的。读取文件的时候，我们就要分别偏移8和16个byte了。



# 下载Mnist数据集
1. 直接到官网下载，下载4个文件
2. 通过代码

下面展示通过代码的方法
``` python
import os 
import os.path
import urllib.request

# mnist的下载地址
mnist_url = 'http://yann.lecun.com/exdb/mnist/'

# mnist的下载文件,用dict形式来存储。后续处理文件会比较方便点
mnist_file = {
  'tr-img':'train-images-idx3-ubyte.gz',  # 训练图片
  'tr-lbl':'train-labels-idx1-ubyte.gz',  # 训练标签
  'te-img':'t10k-images-idx3-ubyte.gz',   # 测试图片
  'te-lbl':'t10k-labels-idx1-ubyte.gz',   # 测试标签
}

# 当前文件的文件夹地址
py_dir = os.path.dirname(os.path.abspath(__file__))
# dataset的文件夹地址,把mnist数据都放入dataset文件夹中
ds_dir = py_dir + '/dataset'

# 放到类里面，先实现下载
class JhuMnist: 
  def __init__(self):
    self._mnist_download()  # 下载数据集

  def _mnist_download(self):
    """下载mnist数据集"""
    # 创建下载的文件夹
    if not os.path.exists(ds_dir):
        os.makedirs(ds_dir)

        # 循环遍历下载文件
        for f in mnist_file.values():
            f_path = ds_dir + '/' + f
            if os.path.exists(f_path):
                continue
            print(f'开始下载{f} ...')
            urllib.request.urlretrieve(mnist_url + f, f_path)
        print('Done')        


if __name__ == '__main__':
  j = JhuMnist()
```

第一次下载的话会比较慢，下载完成后你会发现新建立了一个dataset文件夹，里面有4个文件：
* train-images-idx3-ubyte.gz
* train-labels-idx1-ubyte.gz
* t10k-images-idx3-ubyte.gz
* t10k-labels-idx1-ubyte.gz


# 转换数据到Numpy中
用代码来解释，这里代码我只写出新增或修改的部分
``` python
import gzip         # gzip的解压缩
import numpy as np  # Numpy导入

class JhuMnist:
  def __init__(self):
    self._mnist_download()  # 下载数据集
    self._convert_numpy()   # Numpy转换

  def _read_mnist(self,type,f_path):
    """
    读取mnist文件
    input:
    - type:   读取的文件类型('lbl':标签，'img':图片)
    - f_path: 读取的文件路径
    output:
    - numpy array 数组
    """
    offset = 8 if type == 'lbl' else 16 # 标签正文移动8，图片移动16 

    with gzip.open(f_path,'rb') as f:
      ds = np.frombuffer(f.read(), np.uint8, offset=offset)

    """
    注意如果这里对图片不重新reshape的话,那么图片的ds.shape就是
    训练:(10000*28*28,)  而不是(60000,28*28)
    测试:(10000*28*28,)  而不是(10000,28*28)
    reshape(-1,28*28) 等同于 reshape([-1,28*28])
    设置为-1代表让系统自己去计算,等同于reshape(60000,28) or (10000,28)
    """  
    if type == 'img': 
      ds = ds.reshape(-1,28*28)
    return ds

  def _convert_numpy(self):
    """转换数据集变成Numpy类型"""
    self.ds = {} # 将内容都放入到dict
    for i in mnist_file:
      type = 'img' if i.find('img') != -1 else 'lbl' # 根据标签的key设置文件类型,因为要根据文件类型来偏移
      self.ds[i] = self._read_mnist(type,ds_dir+'/'+mnist_file[i])


# 这个时候你已经可以通过 j.ds获取到各个数据集的numpy内容了
if __name__ == '__main__':
  j = JhuMnist()
```

# 把Numpy数据保存起来
我们看到，我们使用numpy的数据其实是通过gzip->numpy的，那么如果每次我们都要gzip解压，转换numpy数据就比较麻烦，所以大家都通过pickle工具来保存训练好的一些numpy数据
老样子，用代码来解释，这里代码我只写出新增或修改的部分
``` python
import pickle

# pkl文件的地址
pkl_file = py_dir + '/' + 'mnist.pkl'

class JhuMnist:
  def __init__(self):
    # 如果已经有pkl文件了就不再去下载转换保存了
    if not os.path.exists(pkl_file):
      self._mnist_download()  # 下载数据集
      self._convert_numpy()   # Numpy转换
      self._save_as_pkl()     # 保存pkl

  def _save_as_pkl(self):
    with open(pkl_file, 'wb') as f:
        pickle.dump(self.ds, f, -1)
    print('Done')

  def load_mnist(self) -> dict:
    with open(pkl_file,'rb') as f:
      ds = pickle.load(f)
    return ds

# 通过Load方法就可以快速的读取数据
if __name__ == '__main__':
  j = JhuMnist()
  ds = j.load_mnist()
```

# 展示图片数据
用matplotlib可以展示图片,老样子，用代码来解释，这里代码我只写出新增或修改的部分
``` python
import matplotlib.pyplot as plt 

if __name__ == '__main__':
  j = JhuMnist()
  ds = j.load_mnist()
  # 读取训练集的第一个数据，因为图片展示是2维的数据，所以需要在做一次reshape
  img = ds['tr-img'][0].reshape(28, 28) 
  fig =plt.figure()
  plt.axis('off')
  plt.imshow(img, cmap='gray')
  plt.show()
```

OK，mnist的数据就讲完了，是不是很简单～
