### Scrapy下载图片

#### spider中的代码

```python
import scrapy
from game.items import GameItem

class SpiderSpider(scrapy.Spider):
    name = 'spider'
    allowed_domains = ['OTJnYW1lLm5ldCcsJ2ltZy45MmRlbW8uY29tOjgwMTA=']
    start_urls = ['aHR0cDovL216aXR1LjkyZ2FtZS5uZXQvcGFnZS9pbmRleF8zLmh0bWw=']

    #解析封面列表链接
    def parse(self, response):
        covers = response.xpath('//div[@class="postlist"]/ul//li/a/@href').getall()
        for cover in covers:
            #把每一个封面链接传递给self.get_img_count去解析
            yield scrapy.Request('aHR0cDovL216aXR1LjkyZ2FtZS5uZXQ='+cover, callback=self.get_img_count)
        next_cover_page = response.xpath('//div[@class="pagenavi"]/a[last()]/@href').get()
        #限制爬取50页
        if 50 < int(next_cover_page.split('_')[-1].split('.')[0]):
            return None
        else:
            #下一个封面列表再传递给self.parse解析
            yield scrapy.Request('aHR0cDovL216aXR1LjkyZ2FtZS5uZXQ='+next_cover_page, callback=self.parse)

    #解析图片列表链接
    def get_img_count(self, response):
        img = response.xpath('//div[@class="main-image"]//img/@src').get()
        img = img.replace('ZmlsZS45MmdhbWUubmV0JywnaW1nLjkyZGVtby5jb206ODAxMA==')
        name = response.xpath('//h2/text()').get()
        next_img_page = response.xpath('//div[@class="pagenavi"]/a[last()]/span/text()').get()
        imgs = []       #item中的image_urls必须是列表格式。
        imgs.append(img)
        item = GameItem()
        item['image_urls'] = imgs
        item['name'] = name
        # print('打印item内容',item)
        yield item
        #判断当前组图是否终止
        if next_img_page is '下一组»':
            return None
        else:
            next_img_page = response.xpath('//div[@class="pagenavi"]/a[last()]/@href').get()
            yield scrapy.Request('aHR0cDovL216aXR1LjkyZ2FtZS5uZXQ='+next_img_page, callback=self.get_img_count)
```

#### items中的代码

```python
import scrapy

class GameItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    image_urls = scrapy.Field()
    name = scrapy.Field()
```

#### pipelines中的代码

```python
import scrapy
from scrapy.pipelines.images import ImagesPipeline
import os
from game.settings import IMAGES_STORE

class GamePipeline(object):
    def process_item(self, item, spider):
        return item

class ImageDownPipelines(ImagesPipeline):
    #重写父类这个方法，把图片链接绑定上对应的名字。
    def get_media_requests(self, item, info):
        print('判断是否请求图片链接')
        for img in item['image_urls']:
            yield scrapy.Request(img, meta={'name':item['name']})
	
    #重写父类这个方法，给图片指定保存的路径。
    def file_path(self, request, response=None, info=None):
        image_store = IMAGES_STORE
        name = request.meta['name']
        ext = request.url.split('/')[-1]
        filename = os.path.join(image_store, name, ext)
        print('打印图片保存路径',filename)
        return filename
```

#### settings中的代码

```python
ROBOTSTXT_OBEY = False
DEFAULT_REQUEST_HEADERS = {
    	#不带referfer会出现下载图片返回http403代码
        'referer': 'aHR0cDovL216aXR1LjkyZ2FtZS5uZXQveGluZ2dhbi8yOTIzXzMuaHRtbA==',
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'
    }
IMAGES_STORE = 'D:\Images'
ITEM_PIPELINES = {
    #告诉程序执行我自己写的ImageDownPipelines类，而不是执行默认的。
   'game.pipelines.ImageDownPipelines': 1,
}
```

