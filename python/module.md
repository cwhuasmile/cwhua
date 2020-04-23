# lxml库 解析HTML代码

## 1.使用 lxml 的 etree 库

我们可以利用他来解析HTML代码，并且在解析HTML代码的时候，如果HTML代码不规范，他会自动的进行补全。示例代码如下：

```python
from lxml import etree 
text = '''
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a> # 注意，此处缺少一个 </li> 闭合标签
     </ul>
 </div>
'''
#利用etree.HTML，将字符串解析为HTML文档
html = etree.HTML(text) 
# 按字符串序列化HTML文档
result = etree.tostring(html) 
print(result)
```

> etree.HTML()返回的html可直接使用xpath提取标签元素

输入结果如下：

```
<html><body>
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
</ul>
 </div>
</body></html>
```

可以看到。lxml会自动修改HTML代码。例子中不仅补全了li标签，还添加了body，html标签。

## 2. 从文件中读取html代码

除了直接使用字符串进行解析，lxml还支持从文件中读取内容。我们新建一个hello.html文件：

```
<!-- hello.html -->
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
```

然后利用`etree.parse()`方法来读取文件。示例代码如下：

```python
from lxml import etree

# 读取外部文件 hello.html
html = etree.parse('hello.html')
result = etree.tostring(html, pretty_print=True)

print(result)
```

输入结果和1中的相同。

> etree.parse()返回的html可以直接使用xpath提取标签元素。

# 发送电子邮件

```python
import smtplib
from email.mime.text import MIMEText
from email.utils import formataddr

def mail(content=None):
    my_sender='wenxxxxxxx@sina.com'    # 发件人邮箱账号
    my_pass = 'xxxxxxxxxxxx'              # 发件人邮箱密码
    my_user='xxxxxxxxx@sina.com'      # 收件人邮箱账号，我这边发送给自己
    if not content:
        content = "这是一封来自小华仔的测试邮件"
    ret=1
    try:
        msg=MIMEText(content,'plain','utf-8')      # 邮件内容，邮件格式，邮件编码
        msg['From']=formataddr(["小华仔",my_sender])  # 括号里的对应发件人邮箱昵称、发件人邮箱账号
        msg['To']=formataddr(["NickName",my_user])              # 括号里的对应收件人邮箱昵称、收件人邮箱账号
        msg['Subject']="请看正文内容"                # 邮件的主题，也可以说是标题
 
        server=smtplib.SMTP_SSL("smtp.sina.com", 465)  # 发件人邮箱中的SMTP服务器，端口是25
        server.login(my_sender, my_pass)  # 括号中对应的是发件人邮箱账号、邮箱密码
        server.sendmail(my_sender,[my_user,],msg.as_string())  # 括号中对应的是发件人邮箱账号、收件人邮箱账号、发送邮件
        server.quit()  # 关闭连接
    except Exception:  # 如果 try 中的语句没有执行，则会执行下面的 ret=False
        ret=0
    return ret

ret=mail(content="我再测试一封邮件")
if ret:
    print("邮件发送成功")
else:
    print("邮件发送失败")
```

