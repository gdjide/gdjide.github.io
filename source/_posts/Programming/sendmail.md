---
title: python代码实现邮件的发送
categories: 
    - [学习备忘笔记]
single_column: true
cover: https://pic-bed-6vd.pages.dev/img/fm1.webp
banner:
  type: img
  bgurl: https://pic-bed-6vd.pages.dev/img/bg12.webp
  banner_text: 
---
# python代码实现邮件的发送

## 1. 邮件发送的原理

邮件发送的原理是：邮件发送者将邮件发送到邮件服务器，邮件服务器再将邮件转发到邮件接收者。

## 2. 邮件发送的步骤

1. 邮件发送者将邮件发送到邮件服务器。
2. 邮件服务器将邮件转发到邮件接收者。

## 3. 邮件发送的代码实现

```python
import smtplib
from email.mime.text import MIMEText
from email.header import Header


# 邮件发送者的邮箱地址和授权码
sender = 'sender@example.com'
password = 'password' #填入自己的授权码，注意非邮件密码

# 邮件接收者的邮箱地址
receiver = 'receiver@example.com'

# 邮件主题
subject = '邮件主题'

# 邮件内容
content = '邮件内容'

# 创建一个MIMEText对象，用于构造邮件内容
msg = MIMEText(content, 'plain', 'utf-8') # plain:文本类型
# 设置邮件主题
msg['Subject'] = Header(subject,'utf-8')
# 设置邮件发送者
msg['From'] = sender
# 设置邮件接收者
msg['To'] = receiver

# 发送邮件
try:
    # 连接SMTP服务器
    smtpObj = smtplib.SMTP('smtp.example.com', Port) # SMTP服务器的地址和端口
    smtp.helo('smtp.example.com') # 向SMTP服务器标识自己
    smtp.ehlo('smtp.example.com') #协商更多的SMTP扩展
    # 登录SMTP服务器
    smtpObj.login(sender, password)
    # 发送邮件
    smtpObj.sendmail(sender, receiver, msg.as_string())
    print('邮件发送成功')
    # 关闭SMTP服务器连接
    smtpObj.quit()
except smtplib.SMTPException as e:
    print('邮件发送失败', e)
```

## 4. 一些常见邮箱的参数
| 邮箱 | SMTP服务器地址 | 端口 | SSL/TLS加密 | STARTTLS加密 |
| --- | --- | --- | --- | --- |
| QQ邮箱 | smtp.qq.com | 465 | 是 | 是 |
| 163邮箱 | smtp.163.com | 465 | 是 | 是 |
| Gmail | smtp.gmail.com | 465 | 是 | 是 |
| Outlook | smtp.office365.com | 587 | 是 | 是 |
| iCloud | smtp.mail.me.com | 465 | 是 | 是 |


## 5. 邮件发送的注意事项

1. 邮件发送者的邮箱地址和密码需要正确。
2. 邮件接收者的邮箱地址需要正确。
3. 邮件主题和内容需要正确。
4. 邮件服务器需要正确。