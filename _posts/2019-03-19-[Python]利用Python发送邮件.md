---
layout: post
title:  "[Python]利用Python发送邮件"
date: 2019-03-19 16:58:37 +0800
categories: [coding]
---

# 环境准备说明
使用python3，用到了如下2个模块
  - smtplib：负责发送邮件
  - email：负责构造邮件

`发送邮件使用的是SMTP协议，接受邮件则有POP3和IMAP协议`

# 发送邮件
## 事前准备
发送邮件之前，需要具备的一些信息
1. 邮件服务器，端口。（邮件服务器即SMTP服务器，端口，根据RFC协议，SMTP的默认端口为：25）  
`由于网络安全传输的原因，现在大多数SMTP均采用了SSL的链接方式，SSL的默认端口为465.`
2. 登陆用户名和密码
3. 邮件内容
  - 发送方邮件（From）
  - 收件方邮件（To，Cc，Bcc） 
  - 邮件标题
  - 邮件正文
  - 附件

## 发送邮件
1. 连接和登陆服务器
  ``` python
  import smtplib
  #如果使用SSL，采用SMTP_SSL
  #ssl可以用Boolean值来表示是否采用SSL，func代表创建SMTP对象用哪个方法
  func = smtplib.SMTP_SSL if ssl else smtplib.SMTP
  mysmtp = func(host,port)  #创建了mysmtp的smtp对象
  mysmtp.login(username,pwd) #登陆smtp服务器
  ```
  上述代码：host我实际用的值：smtp.exmail.qq.com,port就是465
  username是：xxx@QQ.com pwd是我的密码。  
  `smtp服务器的地址你需要登陆相关邮件服务去找`
  
2. 设置邮件内容
  ``` python
  from email.mime.multipart import MIMEMultipart #构造邮件内容
  from email.utils import formataddr #构造发件人和收件人邮件地址和名称
  from email.header import Header #构造邮件标题使用
  from email.mime.text import MIMEText #构造邮件正文
  from email.mime.base import MIMEBase #构造邮件附件
  
  mime = MIMEMultipart() #构造一个Mime供邮件内容使用！

  #设置邮件标题
  mime['Subject'] = Header('邮件Title','utf-8') #设置title名称和编码
  
  #formataddr会将邮件地址和名称自动处理为name<mailaddr@xx.com>的形式
  #如果不填写邮件名称，那么默认显示邮件地址
  mime['From'] = formataddr('myname','myaddr@addr.com')
  #同理To和Cc和Bcc也是一样的，Bcc没有的化就不设置
  mime['To'] = formataddr('myname','myaddr@addr.com')
  mime['Cc'] = formataddr('myname','myaddr@addr.com')
  mime['Bcc'] = formataddr('myname','myaddr@addr.com')
  '''
  因为From只会有1个，而to，cc，bcc可能会有多个，那么怎么设置呢？
  实际上，在SMTP协议中，多个收件人地址是通过逗号来分割的，因此，
  只需要将多个地址用逗号分隔即可解决该问题！
  '''
  #多地址用一个lst来存放，然后将lst里所有的内容用逗号拼接
  for (name,addr) in mailaddr_list:
    lst.append(formataddr(name,addr))
  to_list_str = ','.join(lst)
  mime['To'] = to_list_str

  #邮件内容设置 邮件内容类型有plain，html等，这里默认使用html，编码还是utf-8
  mime.attch(MIMEText('邮件内容','html','utf-8'))
  #附件内容设置附件内容读取本地文件
  att = MIMEText(open('c://attch.png','rb').read(), 'base64', 'utf-8')
  att["Content-Type"] = 'application/octet-stream'
  att["Content-Disposition"] = 'attachment; filename='+'附件名'
  att.add_header('Content-ID', '1')
  mime.attach(att)

  #发送邮件mysmtp就是上面通过SMTP_SSL生成的对象,然后设置收发件人的邮箱地址
  mysmtp.sendmail('sender@mail.com', ['recv@mail.com','recv2@mail.com'], mime.as_string())

  #发送完成后关闭
  mysmtp.quit()
  ```

我自己封装了一个hmail的类，大家有兴趣可以去看看：
https://github.com/cantahu/hlib/blob/master/hmail.py
 
