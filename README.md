# What's this?

小华仔创建的第一个博客，内容还在不断完善中~~~

## 准备写什么内容?

1，电脑和局域网的一些技术，之前做过这方面的工作。

2，Python相关技术，我目前在学习这方面的知识。

3，前端技术，可能比较渣，搭建这个网页让我学到了一些东西。

4，日常琐碎，让我瞬间顿悟的知识。

5，商业方法应用技巧等等等等。

## 以下是示例文档

- 🚀 小火箭
- ⚡️️ 闪电
- 💎 钻石
- 🔥 燃烧的火焰
- 📼 磁带
- ⏱ 时钟

## 示例代码

一段爬虫代码，地址我就不放了。。。

```python
import requests
from bs4 import BeautifulSoup
import os
import time
import random

#获取页面
def get_html(url):
    headers = {'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8', 
        'accept-encoding': 'gzip, deflate, br', 
        'accept-language': 'zh-CN,zh;q=0.9', 
        'referer': 'https://url地址/', 
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'}
    try:
        r = requests.get(url,headers=headers)
    except:
        write_log(f'{url}网页请求失败')
        return None
    else:
        return r

#提取主页码索引序号和对应的名字
def get_num_name(html):
    num_name = {}
    soup = BeautifulSoup(html.text,"html.parser")
    li = soup.find_all('ul',id='pins')[0].find_all('li')
    for x in li:
        num = x.a.attrs['href'].split('/').pop()
        name = x.a.img.attrs['alt']
        num_name[num] = name
    return num_name

#创建目录
def makepath(num_name):
    false_path = []
    for n,m in num_name.items():
        try:
            os.mkdir(rf'C:\Users\xiaohuazai\Desktop\test\mzitu\{n}')
        #目录创建后失败写入日志
        except:
            false_path.append(n)
            write_log(rf'{n}目录已被创建')
        else:
            with open(rf'C:\Users\xiaohuazai\Desktop\test\mzitu\{n}\{m}.txt','w') as f:
                f.write(f'{n}{m}')
            get_second_link(n,m)
    return false_path

#提取二级页面索引码，图片链接
def get_second_link(num,name):
    url = 'https://url地址/' + num
    html = get_html(url)
    soup = BeautifulSoup(html.text,"html.parser")
    maxpage = soup.find('div',class_='pagenavi').contents[-3].span.get_text()
    for page in range(1,int(maxpage)+1):
        html = get_html(url+'/'+str(page))
        soup = BeautifulSoup(html.text,"html.parser")
        pic_url = soup.find('div',class_='main-image').p.a.img.attrs['src']
        s = random.randrange(15,27)
        s = s/10
        time.sleep(s)
        pic_date = get_html(pic_url)
        if not pic_date:
            write_log(f'{num}中的第{page}下载失败')
            continue
        save_pic(pic_date.content,num,page)
    return None

#保存图片
def save_pic(pic,num,page):
    with open(rf'C:\Users\xiaohuazai\Desktop\test\mzitu\{num}\{page}.jpg','wb') as f:
        f.write(pic)
    return None

#日志
def write_log(info):
    l_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
    with open(r'C:\Users\xiaohuazai\Desktop\test\mzitu\log.txt','a') as f:
        f.write(f'{l_time} '+ info + os.linesep)

if __name__=='__main__':
    url = 'https://url地址/page/3/'
    html = get_html(url)
    num_name = get_num_name(html)
    makepath(num_name)
```

## CSS控制页面布局在浏览器上的微调方法

打开浏览器开发者工具（F12按键或者右键`审查元素`），然后点击对应标签。

![click](/content_images/Click.png)

页面元素会高亮显示：

![highlight](/content_images/Highlight.png)

然后在右侧的Styles选项下可以微调元素的各个参数（位置、大小、填充、颜色、字体等等）：

![detail](/content_images/Detail.png)

## UEFI启动修复方法

以下两种方法都是使用bcdboot修复

#### 第一种、指定esp分区：
环境为64位8PE，bios/uefi启动进入下都可以。

* 1，启动64位8PE，并用esp分区挂载器或diskgenuis挂载esp分区
* 2，打开cmd命令行，输入以下命令并运行

```cmd
bcdboot c:\windows /s o: /f uefi /l zh-cn
```

其中：c:\windows 硬盘系统目录，根据实际情况修改

/s o: 指定esp分区所在磁盘，根据实际情况修改

/f uefi 指定启动方式为uefi

/l zh-cn 指定uefi启动界面语言为简体中文

> 注：64位7PE不带/s参数，故7PE不支持bios启动下修复

#### 第二种、不指定esp分区修复：

环境为64位7或8PE，只有uefi启动进入PE才可以

* 1、不用挂载esp分区，直接在cmd命令行下执行：

```cmd
bcdboot c:\windows /l zh-cn
```

其中 c:\windows 硬盘系统目录，根据实际情况修改

/l zh-cn 指定uefi启动界面语言为简体中文

> 注：在8PE中，我们也可以在uefi启动进入pe后，挂载esp分区用第一种方法修复

