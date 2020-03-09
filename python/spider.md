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

   `//ul/li[position()<3]`表示提取最前面的两个`li`标签

4. 路径表达式

   | 表达式   | 描述                                                       |
   | -------- | ---------------------------------------------------------- |
   | nodename | 选取此节点的所有子节点。                                   |
   | /        | 从根节点选取。                                             |
   | //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
   | .        | 选取当前节点。                                             |
   | ..       | 选取当前节点的父节点。                                     |
   | @        | 选取属性。                                                 |

## 5，selenium

- 打开新的标签页

  ```
  driver.execute_script("window.open('https://www.baidu.com')")
  ```

- 切换游标位置（driver.window_handles是个列表，数字表示第几个打开的标签页，从0开始）

  ```
  driver.switch_to_window(driver.window_handles[2])
  ```

- 关闭标签页

  ```
  driver.close()
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

#### 1. `ValueError: Missing scheme in request url: h `报错

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

​	在确定setting.py设置和pipelines.py中的`类名`没有问题时请尝试以下方法：

- 检查网页请求头headers中的referer，此参数不正确会导致请求返回403或者404。
- settings.py中的IMAGES_STORE参数必须设置，然后再去pipelines.py中调用。否则会导致程序不进入pipelines中