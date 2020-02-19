### requests + bs4

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
        'referer': 'aHR0cHM6Ly93d3cubXppdHUuY29tLw==', 
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
            os.mkdir(rf'C:\Users\xiaohuazai\Desktop\test\123\{n}')
        #目录创建后失败写入日志
        except:
            false_path.append(n)
            write_log(rf'{n}目录已被创建')
        else:
            with open(rf'C:\Users\xiaohuazai\Desktop\test\123\{n}\{m}.txt','w') as f:
                f.write(f'{n}{m}')
            get_second_link(n,m)
    return false_path

#提取二级页面索引码，图片链接
def get_second_link(num,name):
    url = 'aHR0cHM6Ly93d3cubXppdHUuY29tLw==' + num
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
    with open(rf'C:\Users\xiaohuazai\Desktop\test\123\{num}\{page}.jpg','wb') as f:
        f.write(pic)
    return None

#日志
def write_log(info):
    l_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
    with open(r'C:\Users\xiaohuazai\Desktop\test\123\log.txt','a') as f:
        f.write(f'{l_time} '+ info + os.linesep)

if __name__=='__main__':
    url = 'aHR0cHM6Ly93d3cubXppdHUuY29tL3BhZ2UvMy8='
    html = get_html(url)
    num_name = get_num_name(html)
    makepath(num_name)
```

### requests单独下载图片并保存

```python
import requests
import os

headers = {
        'referer': 'https://www.runoob.com',
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.106 Safari/537.36'
    }
url = 'https://static.runoob.com/images/mix/v2-854e3df8ea850c977c30cb1deb1f64db_r.jpg'
resp = requests.get(url,headers=headers)
print(resp.status_code) #打印response状态码，查看网页返回数据是否正常
with open('temp1.jpg','wb') as f:
    f.write(resp.content)
```

