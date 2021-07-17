---
title: Python在Ajax中的应用
date: 2018-09-23 09:12:11
tags: [python自动化, Ajax]
categories:
 - python自动化
 - Ajax
---

Ajax技术在网站中的应用随处可见，python要提取网页中的内容，就需要模拟动态的网页加载，对于动态页面信息的爬取，一般分为两种情况，一种是直接从JavaScript中采集加载的数据、需要自己去手动分析Ajax请求来进行信息的采集，另一种则是直接从浏览器中采集已经加载好的数据、即可以使用无界面的浏览器如PhantomJS来解析JavaScript。

这里重点介绍下Python如何模拟Ajax请求采集加载JavaScript的数据，从而获取网页内容 -
<!--more-->

##### 1. 通过给定的url获取页面文本
```
url = r"https://www.toutiao.com/search/?keyword=%E8%A1%97%E6%8B%8D"
req = requests.get(url)
html = req.text

reload(sys)
sys.setdefaultencoding("utf-8")
```

##### 2. 将url源代码保存在htmlfile文件里，方便查看提取内容
```
saveHtmlFile = "C:\Users\Luck_Lello\Desktop\saveHtmlFile"
fo = open(saveHtmlFile + "\htmlFile.html", 'w+')
fo.write(html)
fo.close()
```

##### 3. 通过url得到的html没有需要的图片，分析这些图片是由XHR（Ajar）生成的，所以构造Ajax请求页面，获得需要的图片目标
```
from urllib import urlencode

offset = 20
params = {'offset': offset, 'format': 'json', 'keyword': '街拍', 'autoload': 'true', 'count':'20', 'cur_tab': '1' }
url = "https://www.toutiao.com/search_content/?" + urlencode(params)
print url
```

##### 4. 保存Ajax请求后的页面，然后就可以基于该页面做静态内容的提取了
```
response = requests.get(url)
htmlContent = response.text
fo = open(saveHtmlFile + "\htmlFile.html", 'w+')
fo.write(htmlContent)
fo.close()
```


#### 实例应用
- 从头条街拍信息中提取相应的图片，请看代码实现

  ```
  # try:
  #     response = requests.get(url)
  #     if response.status_code == 200:
  #         getConn = response.json()
  # except requests.ConnectionError:
  #     print "Linkage is not available"

  # import re
  # reg = re.search('\d:\s{url:\s"(.*?)"}', htmlContent, re.S)
  #
  # print reg


  import requests

  def get_page(offset):
      params = {
          'offset': offset,
          'format': 'json',
          'keyword': '街拍',
          'autoload': 'true',
          'count': '20',
          'cur_tab': '1',
      }
      url = 'http://www.toutiao.com/search_content/?' + urlencode(params)
      try:
          response = requests.get(url)
          if response.status_code == 200:
              return response.json()
      except requests.ConnectionError:
          return None

  def get_images(json):
      if json.get('data'):
          for item in json.get('data'):
              title = item.get('title')
              images = item.get('image_detail')
              for image in images:
                  yield {
                      'image': image.get('url'),
                      'title': title
                  }


  import os
  from hashlib import md5


  def save_image(item):
      if not os.path.exists(item.get('title')):
          os.mkdir(item.get('title'))
      try:
          response = requests.get(item.get('image'))
          if response.status_code == 200:
              file_path = '{0}/{1}.{2}'.format(item.get('title'), md5(response.content).hexdigest(), 'jpg')
              if not os.path.exists(file_path):
                  with open(file_path, 'wb') as f:
                      f.write(response.content)
              else:
                  print('Already Downloaded', file_path)
      except requests.ConnectionError:
          print('Failed to Save Image')


  from multiprocessing.pool import Pool
  def main(offset):
      json = get_page(offset)
      for item in get_images(json):
          print(item)
          save_image(item)

  GROUP_START = 1
  GROUP_END = 20

  if __name__ == '__main__':
      pool = Pool()
      groups = ([x * 20 for x in range(GROUP_START, GROUP_END + 1)])
      pool.map(main, groups)
      pool.close()
      pool.join()
  ```
