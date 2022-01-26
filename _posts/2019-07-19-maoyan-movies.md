---
layout: post
title: "【Python】利用requests库和Xpath爬取猫眼电影榜单"
author: "Yingwei Guo"
categories: 
tags: 爬虫
image: 
---
> 博主的前几篇有关定向网络爬虫的博客，在解析HTML界面时，都是运用了BeautifulSoup和re库进行解析，这篇博客写一下怎么用Xpath语法进行HTML界面解析，从而得到我们想要的结果。

## 说明
- 爬取猫眼历史电影榜单，并将结果写入到文件中。
- 放弃使用re和BeautifulSoup，采用Xpath语法进行解析页面。

## 脚本如下

```python
'''
@Author: Guo Yingwei
@Date: 2019-07-12 00:51:35
@E-mail: willgyw@126.com
@Description:  
'''

import requests
from lxml import etree
import codecs
import time

def get_page(url):
    try:
        kv = {'User-Agent':'Mozilla/5.0'}
        r = requests.get(url, headers = kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return '爬取失败'

def parse_page(html, res_list):
    path = etree.HTML(html)
    movie_list = path.xpath('//dl[@class="board-wrapper"]/dd')
    for movie in movie_list:
        index = movie.xpath('i/text()')[0]
        title = movie.xpath('a/@title')[0]
        score = movie.xpath('div/div/div[@class="movie-item-number score-num"]/p/i/text()')
        scores = ''.join(score)
        res_list.append([index, title, str(scores)])

def write_res(res_list):
    tplt = u'{0:<5}\t{1:{3}^16}\t{2:{3}^5}'
    with codecs.open('MaoyanMovies100.txt', 'wb',  encoding='utf-8') as f:
        f.write(tplt.format('排名', '名称', '评分', chr(12288)) + '\n')
        for i in res_list:
            f.write(tplt.format(i[0], i[1], i[2], chr(12288)) + '\n')

def main():
    start_url = 'https://maoyan.com/board/4'
    movie_list = []
    for i in range(10):
        html = get_page(start_url + '?offset=' + str(10*i))
        parse_page(html, movie_list)
    write_res(movie_list)
    time.sleep(1)   

if __name__ == '__main__':
    main()
```


## 结果如下

![爬去结果](https://img-blog.csdnimg.cn/20190718210159933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjM1ODA3Nw==,size_16,color_FFFFFF,t_70)



另外，由于时间和水平有限，难免会有不妥之处，欢迎交流。


（此文章所有内容仅用于交流和学习，如有不当，还请联系博主删除。）