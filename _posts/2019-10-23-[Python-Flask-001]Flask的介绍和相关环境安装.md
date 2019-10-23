---
layout: post
title: "[Python-Flask-001]Flask的介绍和相关环境安装"
date: 2019-10-23 10:37:44
categories: [coding]
---

# 虚拟环境
`保证环境的干净，不和系统的包冲突，同时使用虚拟环境不需要管理员权限`
## 虚拟环境的安装
使用到的命令 **virtualenv** 如果没有virtualenv命令，请安装

```
mkdir jhuflask  # 今后项目放在该文件夹中，所以虚拟环境也在这里创建
cd jhuflask
virtualenv venv
```
创建完成后，会出现一个venv的文件夹，里面放的是虚拟环境的相关文件

## 使用虚拟环境
在jhuflask下面执行
```
source venv/bin/active
. venv/bin/active       # 这个命令和上面那个命令是一样的
```
成功执行后，你的命令行前线会出现(venv) 这样的标识，代表你已经启动激活了虚拟环境
推出虚拟环境使用
```
deactive
```
退出后，(venv)标识消失，代表已经推出虚拟环境


# 初识Flask
` `
## pip安装flask程序
在激活虚拟环境的状态下，执行pip安装
```
(venv)pip install flask
```

## 建立一个基本的Flask程序
创建一个app.py的文件（Flask的默认启动程序是app，所以这里暂时先叫app.py）
``` python
# 引入Flask类
from flask import Flask

# 初始化
app = Flask(__name__)

@app.route('/')   # 这个装饰器在flask中称为路由
def index():      # 这个函数在flask中称为视图函数
  return 'hello flask',200 # 这个返回被称为响应, 200是HTTP的响应码，可以省略
  # return 'hello flask' 这个和上面那句Return是同样的效果

if __name__ == '__main__':
  # 启动Flask服务，其中debug=True代表为调试模式
    app.run(debug=True)
```

## 运行Flask程序
有多种方式可以运行flask
* python app.py
* flask run
成功运行后，输入127.0.0.1:5000就可以访问到自己的网页了

关于命令flask run： 这个会去找环境变量FLASK_APP的值，如果找不到会去找当前目录下的app.py 或者wsgi.py,所以如果我们把文件名从app.py 改成 jhweb.py，那么就会出现找不到相关的模块。（python jhweb.py还是可以执行的)  
解决方法就是 export FLASK_APP=jhweb 这样就可以再次运行了。

## 安装python-dotenv
通过EXPORT的方法是临时的环境变量，如何设置一次，一劳永逸呢？  
```
pip install python-dotenv
```
安装完成后，建立一个.flaskenv的文件，写入内容FLASK_APP=jhweb 这样就完成了环境变量的配置了
