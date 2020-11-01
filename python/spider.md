## 1，urllib

## 2，requests

### Requests保持cookie操作

```python
import Requests

url = "xxxx"
data = {
    name: "xxxx",
    pwd: "xxxx"
}
headers = {
    'User-Agent': "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36"
}

session = requests.Session()

session.post(url, data=data, headers=headers)

response = session.get('url')
with open('xxx.html', 'w', encoding='utf-8') as f:
    f.write(response.text)
```

### post发送json数据支持中文不乱码的设置方法

在requests源码的models.py文件中，找到prepare_body函数。在comlexjson.dumps(json)里加个参数ensure_ascii=False.
另外还可添加separators=(',', ':')参数，取消json格式化后多余的空格；添加indent参数指定缩进的空格数（参数为0表示只换行不缩进）

转自：https://www.cnblogs.com/Simple-Small/p/10006580.html



## 3，BeautifulSoup

## 4，xpath

1. xpath 获取标签内的 text ， href

   - `/li/a/@herf` 这样取的是href的内容
   - `/li/a/text()` 这样取的是text内容
   
2. 指定标签含有那个属性

   ```html
   <div class="tab_a" id="tab_b">......</div>
   ```

   上面标签可以用`//div[@class="tab_a"]`提取

3. 指定条件

   ```html
   <ul id="pins">
               <li>......</li>
               <li>......</li>
               <li>......</li>
               <li>......</li>
               <li>......</li>
               <li>......</li>
   </ul>
   ```

   `//ul/li[1]`表示提取第一个`li`标签

   `//ul/li[last()]`表示提取最后一个`li`标签

   `//ul/li[last()-1]`表示提取倒数第二个`li`标签

   `//ul/li[position()《3]`表示提取最前面的两个`li`标签

4. 路径表达式

   | 表达式   | 描述                                                       |
   | -------- | ---------------------------------------------------------- |
   | nodename | 选取此节点的所有子节点。                                   |
   | /        | 从根节点选取。                                             |
   | //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
   | .        | 选取当前节点。                                             |
   | ..       | 选取当前节点的父节点。                                     |
   | @        | 选取属性。                                                 |
   
5.  class中含有多个值的定位方法

    ```html
    <div class="a b" id="tab">......</div>
    ```

    上面的内容用`//div[@class="a"]`是无法定位到的。必须用`//div[contains(@class,"a")]`才能定位到
    
6. xpath不能定位表格`tbody`标签，如`//table/tbody/tr[3]/td/text()`无法定位表格，可以把其中的`tbody`去掉，改为定位`tbody`下面的子标签`//table//tr[3]/td/text()`

## 5，selenium

- 打开新的标签页

  ```
  driver.execute_script("window.open('https://www.baidu.com')")
  ```

- 切换游标位置（driver.window_handles(返回所有窗口句柄)是个列表，数字表示第几个打开的标签页，从0开始）

  ```
  driver.switch_to_window(driver.window_handles[2])
  ```

- 关闭标签页

  ```
  driver.close()
  ```

```python
br = webdriver.Chrome('C:\Program Files (x86)\Google\Chrome\Application\chromedriver.exe') #启动chrome浏览器，在path中可以不代参数
element = driver.find_element_by_id("coolestWidgetEvah")   #查找这个内容的id标签<div id="coolestWidgetEvah">...</div>
cheeses = driver.find_elements_by_class_name("cheese")    #按class名查找<div class="cheese"><span>Cheddar</span></div><div class="cheese"><span>Gouda</span></div>
frame = driver.find_element_by_tag_name("iframe")#DOM标签元素的名称。<iframe src="..."></iframe>
cheese = driver.find_element_by_name("cheese")#找到具有匹配name属性的input元素。<input name="cheese" type="text"/>
cheese = driver.find_element_by_link_text("cheese")#找到具有匹配可见文本的link元素。<a href="http://www.google.com/search?q=cheese">cheese</a>>
cheese = driver.find_element_by_partial_link_text("cheese")#找到部分匹配可见文本的链接元素。<a href="http://www.google.com/search?q=cheese">search for cheese</a>>
cheese = driver.find_element_by_css_selector("#food span.dairy.aged")#css选择器查找<div id="food"><span class="dairy">milk</span><span class="dairy aged">cheese</span></div>
inputs = driver.find_elements_by_xpath("//input")#通过xpath查找

#获取文本值
element = driver.find_element_by_id("element_id")
element.text       #请注意，这只会返回页面上显示的可见文本

driver.find_element_by_id("submit").click()    #点击"submit"按钮
#前进和后退
driver.forward()
driver.back()

# find_element_by_name 通过name查找单个元素
# find_element_by_xpath 通过xpath查找单个元素
# find_element_by_link_text 通过链接查找单个元素
# find_element_by_partial_link_text 通过部分链接查找单个元素
# find_element_by_tag_name 通过标签名称查找单个元素
# find_element_by_class_name 通过类名查找单个元素
# find_element_by_css_selector 通过css选择武器查找单个元素

#获取元素信息
btn_more = browser.find_element_by_id('btn_more')
print(btn_more.get_attribute('class')) # 获取属性
print(btn_more.get_attribute('href')) # 获取属性
print(btn_more.text) # 获取文本值

元素交互操作
btn_more = browser.find_element_by_id('btn_more')
btn_more.click() # 模拟点击,可以模拟点击加载更多
input_search = browser.find_element(By.ID,'q')
input_search.clear() # 清空输入

# 执行JavaScript脚本
browser.execute_script('window.scrollTo(0, document.body.scrollHeight)')
browser.execute_script('alert("To Bottom")')
```



## 6，scrapy框架

### Scrapy快速入门

#### 1.目录结构介绍

1. items.py：用来存放爬虫爬取下来数据的模型。
2. middlewares.py：用来存放各种中间件的文件。
3. pipelines.py：用来将`items`的模型存储到本地磁盘中。
4. settings.py：本爬虫的一些配置信息（比如请求头、多久发送一次请求、ip代理池等）。
5. scrapy.cfg：项目的配置文件。
6. spiders包：以后所有的爬虫，都是存放到这个里面。

#### 2.创建和运行项目

- 创建项目

  ```ptyhon
  scrapy startproject [项目名称]
  ```

- 创建爬虫

  ```python
  scrapy genspider [爬虫名字] [域名]	#项目名称不能和爬虫名称一样
  ```

- 运行项目

  ```python
  scrapy crawl [爬虫名字]		#需要进入到爬虫所在路径
  ```

  如果不想每次都执行命令运行爬虫，可以新建一个*.py文件，填入以下内容，执行该文件即可。

  ```python
  from scrapy import cmdline
  cmdline.execute("scrapy crawl [爬虫名字]".split())
  ```

### settings.py设置注意事项

#### 1. DEFAULT_REQUEST_HEADERS默认请求头

- 该字典内部的键key首字母用小写，最好不用大写。
- referer不能漏，一般填写网站首页域名。

#### 2. ITEM_PIPELINES中间件

- 启动时取消注释，

- 如果要重写pipelines，需要把默认的pipelines注释掉，添加自己的，格式为：

```python
'BAT.pipelines.ImagesnamePipeline':1
```

其中BAT是项目名称，pipelines是BAT文件夹里面的py文件，ImagesnamePipelins是py文件里的class类。数字1表示优先级，1~1000，数值越小优先级越高。

### 问题与分析解决方案

#### 1. ValueError: Missing scheme in request url: h 报错

- 如果单纯获取文本，那么只需start_urls是一个list；而如果获取图片，则必须**start_urls与item**中存储图片路径字段这两者必须都是 list。（保存图片用image_urls）

  list列表用法注意事项：

  ```python
  #下面是错误示范
  str1 = 'baidu.com'
  list1 = list(str1)		#list1实际结果是['b','a','i','d','u','.','c','o','m']
  
  #下面是正确写法
  str1 = 'baidu.com'
  list1 = []
  list1.append(str1)
  ```

#### 2. scrapy不抓取重复的网页解决方法

- scrapy爬虫有时候会对一个网页重复爬取提取不同的数据，这时候会发现，后面的那个重复爬取scrapy直接终止了。原因是scrapy的Request逻辑里面 *dont_filter=False*，也就是重复网页不爬取。需要修改下这个参数：

  ```python
  class crapy.http.Request(url[, callback, method='GET', headers, body, cookies, meta, encoding='utf-8', priority=0, dont_filter=False, errback, flags, cb_kwargs])
   #调用Request时将参数dont_filter设置为True就可以了。
  ```

#### 3.Pipelines中间件不运行（不执行）解决方案

在确定setting.py设置和pipelines.py中的`类名`没有问题时请尝试以下方法：

- 检查网页请求头headers中的referer，此参数不正确会导致请求返回403或者404。
- settings.py中的IMAGES_STORE参数必须设置，然后再去pipelines.py中调用。否则会导致程序不进入pipelines中



## 7，aiohttp

### 1. 发送json数据支持中文不乱码的设置方法

在aiohttp模块的根目录，找到payload.py文件，修改`class JsonPayload(BytesPayload)`这个类下面的super().__init__(dumps(value).encode(encoding),content_type=content_type, encoding=encoding, *args, **kwargs)把其中的`dumps`函数加上参数`ensure_ascii=False`即可

### 2. 异步中使用asyncio.wait()函数创建的任务，其中的单一任务报错不会提示。需要等到全部任务执行完毕才会统一报错

## 8，用websocket控制浏览器

#### 1.原理

在浏览器端启动websocket，在爬虫程序里给浏览器端的websocket发送指令，让浏览器端的websocket执行一些操作(生成加密数据等)，把生成的数据再返回给爬虫程序，此举可省去扣js加密代码的大量工作。

由于浏览器里面是websocket客户端，爬虫程序里也是websocket客户端，所以需要另建一个websocket服务端负责转发这两个客户端的消息。

#### 2.代码及说明

##### 1.服务端

```python
import asyncio
import json
import websockets

STATE = {"value": 0}
USERS = set()

#此函数不严谨，根据自己项目需求修改。
async def notify_state(websocket):#收到一个客户端消息，转发给其他的客户端，
    if len(USERS) > 1:  # 如果客户端数量大于2，把任意客户端收到的信息转发给除自己意外的所有客户端。
        await asyncio.wait([user.send(STATE["value"]) for user in USERS if user != websocket])
    else:	#客户端数量小于2发送给自己。
        await asyncio.wait([user.send(STATE["value"]) for user in USERS])

async def counter(websocket, path):
    try:
        USERS.add(websocket)
        print("已链接客户端数量：", len(USERS))
        async for message in websocket:
            print("收到的信息：", message)
            STATE["value"] = message
            await notify_state(websocket)
    except:
        pass
    finally:
        USERS.remove(websocket)
        print("剩余链接客户端数量", len(USERS))
####################普通连接方式##################
start_server = websockets.serve(counter, "127.0.0.1", 6789)	#主函数，ip，端口
################################################

#####################启用SSL连接方式##############
#ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
#ssl_context.load_cert_chain(r"D:\xxxxx\abc.com.pem")	#pem格式的ca证书路径
#start_server = websockets.serve(counter, "127.0.0.1", 6789, ssl=ssl_context)
#################################################

asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()

```

##### 2.浏览器端

```js
//此函数根据实际情况自己创建
create_singeprint = function (info) {
    //这里执行一顿操作
    return "执行了" + info
}

start_websocket = function () {
    var ws = new WebSocket('ws://abc.com:6789');//如果开启了SSL请用'wss://abc.com:6789'
	var ws.onopen=function () {
			console.log('open')
		}
	var ws.onclose=function () {
			console.log('close')
		}
	var ws.onmessage=function (e) {
            mess = e.data
        	//根据收到的指令内容执行相应的操作
            if (mess == "join_in") {
                info_mess = create_singeprint("join_in")
                ws.send(info_mess)
            }
            else if (mess == "login") {
                info_mess = create_singeprint("login")
                ws.send(info_mess)
            }
            else if (mess == "exit") {
                info_mess = create_singeprint("exit")
                ws.send(info_mess)
            }
        }
    }
//执行此函数时请关闭断点状态，否则websocket是断点状态，并不会启动
create_singeprint()
```

##### 3.爬虫端

```python
import asyncio
import websockets
import time

async def sendinfo(websocket,semaphore):
    async with semaphore:
        for i in range(10):
            info = await q.get()
            await websocket.send(str(info))
            print(f"> {info}")

async def recvinfo(websocket,semaphore):
    async with semaphore:
        for i in range(200):
            inf = await websocket.recv()
            print(inf)

async def hello():
    for i in range(412630,412830):
        await q.put(i)
    uri = "ws://abc.com:6789"
    websocket = await websockets.connect(uri)
    name = input("按任意键开始")
    s = time.time()
    semaphore = asyncio.Semaphore(5) # 限制并发量为5
    to_get = [sendinfo(websocket,semaphore) for i in range(20)] #总共20任务
    to_get.append(recvinfo(websocket,semaphore))
    print("========================")
    await asyncio.wait(to_get)
    print("总共花费秒数：",time.time()-s)

q = asyncio.Queue(999)
asyncio.get_event_loop().run_until_complete(hello())
```

