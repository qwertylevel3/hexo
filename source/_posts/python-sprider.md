title: python_sprider
date: 2016-10-16 19:14:28
tags: python
---
>用python写一个简易爬虫,用来爬取壁纸
<!--more-->

# python爬虫爬取壁纸

## 需求
图片网站网址:http://e-shuushuu.net/

这里面大部分都是acg人物插画,手动从里面找出壁纸并保存到本地略麻烦,遂决定写一个爬虫从里面爬一些壁纸下来

这个网站有个search页面:http://e-shuushuu.net/search/

可以提交一些图片的tags,来缩小需要搜索的范围(不过这些tags似乎不能模糊搜索)

那么现在的需求:
* 爬取图片宽>=1366个像素
* 爬取图片长>=768个像素
* 可以更改tags来爬取不同的图片

## 工具

### Beautiful Soup
"Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式."----摘自官方文档

官方文档:https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html#

安装:
```
pip install beautifulsoup4
```

beautifulsoup可以搭配不同的HTML解析器,这里只用了原生的解析器

可以如下创建一个beautifulsoup对象,这里的index.html为当前路径下的一个网页文件
```
from bs4 import BeautifulSoup

soup=BeautifulSoup(open("index.html"), “html.parser”)
```


## 抓取网页

代码:
```
import os
import requests

import sys
reload(sys)
sys.setdefaultencoding('utf-8')


params = {'source': '"Steins;Gate"'}

indexPage = requests.post("http://e-shuushuu.net/search/process/", data=params)

picCount=0
if not os.path.exists('./temp/'):
    os.mkdir('./temp/')
f=open('./temp/index.html','wb')
f.write(indexPage.text)
f.close()

```

其中这里的
```
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```
是为了防止这个....
```
UnicodeEncodeError: 'ascii' codec can't encode character.....
```

以上代码向网站提交一个表单,获取图片内容为"Steins;Gate"的搜索结果

结果作为一个网页文件保存到index.html里面

如果只是抓取一个网页而不需要提交表单可以直接用
```
page=requests.get(url)
```
的形式

另外,表单提交的目标url可以通过浏览器的开发者工具,查看"form"标签的"action"属性来获得,提交的参数也同理,通常查看"input"的"name"属性,来看具体把什么参数值提交到哪里

这里可以写一个函数,来保存页面
```
def writePage(url,count):
    if not os.path.exists('./pages/'):
        os.mkdir('./pages/')
    page=requests.get(url)
    f=open('./pages/'+str(count)+'.html','wb')
    f.write(page.text)
    f.close()
    print (str(count)+'page saved!')
```
其中count作为一个区别文件名的参数

## 从网页提取图片url

现在我们有了抓取的网页文件,需要把其中的图片的信息提取出来
```

def getPicInfoFromPage(pageFile):
    soup=BeautifulSoup(open(pageFile),"html.parser")
    picInfos=[]
    imageTages=soup.find_all(class_='thumb_image')

    for tag in imageTages:
        href="http://e-shuushuu.net"+tag['href'].__str__()
        name="."+tag['href'].__str__()
        pic=[]
        pic.append(name)
        pic.append(href)
        picInfos.append(pic)

    index=0
    for dl in soup.find_all('dl'):
        for dt in dl.find_all('dt'):
            if dt.string=='Dimensions:':
                dim=dt.next_element.next_element.next_element.string
                split1=dim.find('x')
                split2=dim.find(' ')
                picX=dim[0:split1]
                picY=dim[split1+1:split2]
                picInfos[index].append(int(picX))
                picInfos[index].append(int(picY))
                index=index+1

    return picInfos

```
这里通过使用beautifulsoup来在网页文件中查找图片的信息,最终将其组装成列表
其中每个列表项的第一项为图片名,第二项url,第三项宽度,第四项为长度

这里的解析过程由具体的网页文件格式和各个标签的属性值而定,具体网页具体分析.在这里的网站中获取的每个页面都可用这个来解析

## 保存图片
```
def writePic(picInfo):
    if not os.path.exists('./images/'):
        os.mkdir('./images/')
    conn = urllib.urlopen(picInfo[1])
    f = open(picInfo[0],'wb')
    f.write(conn.read())
    f.close()
    print('Pic Saved!')
```
对于上一步产生的由picInfo组成的picInfos列表,可以根据需要保存其中的某些图片


## 转到下一页
```
def nextPageUrl(page):
    soup=BeautifulSoup(open(page),"html.parser")
    next=soup.find_all(class_='next')
    return "http://e-shuushuu.net/search/results/"+next[0].find('a')['href'].__str__()
```
搜索页面文件中的"next",返回其url

## 将以上组装起来
大体流程就是提交一个表单,保存结果页面,从结果页面中提取图片的url,观察其长宽属性是否满足条件,如果满足就保存下来

如果抓取的图片数量一个页面还不够,可以在跳转到下一页继续上述过程

最终的组装结果在这里:https://github.com/qwertylevel3/PictureSprider



## 吐槽
抓取速度好慢......下次优化一下.....

***

## 参考文献

* https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html#
* https://cuiqingcai.com/1319.html
* http://blog.csdn.net/wudishine/article/details/11528791
