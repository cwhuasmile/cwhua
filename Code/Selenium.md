## selenium多线程下载图片

```python
from selenium.webdriver import Chrome
import requests
from queue import Queue
from threading import Thread
import os

#获取图片链接和title
def set_link(q, start):
    driver = Chrome()
    driver.get(f"aHR0cHM6Ly93d3cudHA4LmNvbS90YWcvZGFkYW5yZW50aXlpc2h1L3tzdGFydH0uaHRtbA==")
    driver.set_window_size(1024, 1024)
    while True:
        covers = driver.find_elements_by_xpath('//div[@class="m-list ml1"]/ul/li/a')
        next_page = driver.find_elements_by_xpath('//div[@class="page"]/a[last()-1]')
        for cover in covers:
            cover.click()
            name = cover.get_attribute('title') #获取<a>标签里的title属性值
            #click点击后会弹出新标签页，将游标切换到新的标签页
            driver.switch_to_window(driver.window_handles[1])
            while True:
                link = driver.find_element_by_xpath("//div[@class='pic-main']/a/img")
                next_img_text = driver.find_element_by_xpath('//div[@class="page"]/a[last()]').text
                img = link.get_attribute("src") 
                item = (name, img)
                q.put(item) #把名字和链接put到队列
                #判断当前组图是否完毕，是则关闭当前标签并把游标切换到主标签页。
                if next_img_text == "下一页":
                    driver.close()
                    driver.switch_to_window(driver.window_handles[0])
                    break
                else:
                    link.click()
        #主页面全部获取完之后点击下一页。
        next_page.click()

#保存图片
def down(q):
    while True:
        item = q.get()
        headers = {
            'referer': 'aHR0cHM6Ly93d3cudHA4LmNvbS8=',
            'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.106 Safari/537.36'
        }
        r = requests.get(item[1], headers=headers)
        print(r.status_code, q.qsize())
        ext = item[1].split('/')
        path = 'D:\\Python_code\\123\\tu8' + os.sep +item[0]
        if not os.path.exists(path):
            os.mkdir(path)
        filename = path + os.sep + ext[-1]
        with open(filename, 'wb') as f:
            f.write(r.content)

q = Queue(100)
t1 = Thread(target=set_link, args=(q,2))
t2 = Thread(target=set_link, args=(q,4))
t3 = Thread(target=set_link, args=(q,6))
t4 = Thread(target=down, args=[q])  #只添加了一个保存图片的线程，因为保存图片太快，获取图片链接慢。
t1.start()
t2.start()
t3.start()
t4.start()
```

