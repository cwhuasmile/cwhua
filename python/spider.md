## 1，urllib

## 2，requests

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

## 6，scrapy框架

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