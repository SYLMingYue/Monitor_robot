*在这个文档中，我将建立一个监视velodrome池子的监视机器人，实现的目标为能够监视lp池子动态，将机器人部署到云服务器上，并在价格发生异动时发出提醒*
## 1 通过geckoterminal获取Velodrome中的$MET代币价格
```
import requests
import json

# 定义API的URL
url = "https://api.geckoterminal.com/api/v2/networks/"
url1 = "https://api.geckoterminal.com/api/v2/networks/optimism/pools/0xa8e4fa96327c5a93e159bd781f3ccfd860519d82"
# 发送GET请求
response = requests.get(url1)

# 检查响应状态码，如果状态码为200，则请求成功
if response.status_code == 200:
    # 获取响应数据
    data = response.json()  # 如果返回的是JSON格式的数据
    # 或者可以使用response.text来获取原始文本响应

    # 处理响应数据
    print(data)
    METprice = data['data']['attributes']['base_token_price_usd']
    result = METprice[:6]
    print(result)
    # 在这里，您可以根据API的返回数据格式，提取需要的信息或进行其他操作

else:
    print("请求失败，状态码：", response.status_code)

```

## 2 通过向邮箱发送电子邮件提醒
```
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

def send_email(subject, body, recipient_email):
    # 163邮箱的邮件服务器设置
    smtp_server = "smtp.163.com"
    smtp_port = 25
    sender_email = 'Wdp_bot@163.com'  # 请替换为您的163邮箱地址
    sender_password = "***"  # 请替换为您的163邮箱密码或授权码             /需要修改

    # 创建邮件内容
    message = MIMEMultipart()
    message["From"] = sender_email
    message["To"] = recipient_email
    message["Subject"] = subject
    message.attach(MIMEText(body, "plain"))

    # 连接邮件服务器并发送邮件
    try:
        with smtplib.SMTP(smtp_server, smtp_port) as server:
            server.starttls()  # 开启TLS加密
            server.login(sender_email, sender_password)
            server.sendmail(sender_email, recipient_email, message.as_string())
        print("邮件发送成功！")
    except Exception as e:
        print("邮件发送失败:", str(e))

# 示例使用：发送警报邮件
data = 0.75
if data < 1:
    subject = "警报：数据小于1！"
    body = "警报：数据小于1，请注意！"
    recipient_email = "***"  # 请替换为接收警报邮件的邮箱地址             /需要修改
    send_email(subject, body, recipient_email)
```

## 3 设置数据判断算法
```
MET_OP = float(1)
MET_ETH = float(1.5)
pricegap = (MET_ETH - MET_OP)/MET_OP*100 #pricegap变量是百分比
```

## 4 设置循环程序
```
import time

def your_function():
    # 在这里编写您需要执行的代码
    print("这是每过5分钟执行的代码")

# 主程序循环
while True:
    # 调用您的函数
    your_function()

    # 让程序休眠5分钟
    time.sleep(5 * 60)  # 5分钟的秒数

# 程序将会一直运行，并且每过5分钟执行一次 your_function()
```
