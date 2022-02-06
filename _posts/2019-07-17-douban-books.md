---
layout: post
title: "【Python】利用requsets、bs4、re库爬取豆瓣图书top250"
author: "Yingwei Guo"
categories: 
tags: 爬虫
image: 
---

>因为最近在练习定向网络爬虫技术，爬了豆瓣电影之后，顺便爬一下豆瓣图书，具体请看介绍。

## 介绍
1.用到的库有requests，bs4中的BeautifulSoup，以及正则表达式re库。requests用来获取网页，BeautifulSoup用来解析页面，re用来匹配查找。
2.值得注意的是，博主想把top250的图书的名字、评分、一句话简介提取出来，但是没有看到**有的图书是没有一句话简介的**，于是刚开始的时候疯狂报错。严谨考虑确实应该加一层判断才对。
3.其实re库和BeautifulSoup库都不是必须的，使用其中一个就可以达到效果，这里为了练习，两个库都使用上了。
## 脚本如下

```python
'''
@Author: Guo Yingwei
@Date: 2019-07-09 17:02:37
@E-mail: willgyw@126.com
@Description:  crawl top250 book on Douban.
'''
import requests
from bs4 import BeautifulSoup
import codecs
import re

def get_page(url):
    try:
        r = requests.get(url)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ''
    
def parse_html(html, return_list):
    soup = BeautifulSoup(html, 'html.parser')
    book_list = soup.find('div', attrs={'class': 'indent'}).find_all('tr')
    for book in book_list:
        book = str(book)
        title = re.search(r'(?<=title=").*?(?=">)', book).group()
        rating_num = re.search(r'(?<="rating_nums">).*?(?=</span>)', book).group()
        if re.search(r'(?<=<span class="inq">).*?(?=</span>)', book):
            inq = re.search(r'(?<=<span class="inq">).*?(?=</span>)', book).group()
        else:
            inq = '暂无介绍'
        return_list.append([title, rating_num, inq])
    
def write_file(return_list):   
    with codecs.open('doubanBookTop250.txt','wb+', encoding='utf-8') as f:
        tplt = '{0:<4}\t{1:{4}^16}\t{2:{4}^10}\t{3:<20}'
        f.write(tplt.format('排名', '名称', '评分', '一句话简介', chr(12288)) + '\n')
        count = 0
        for i in return_list:
            count += 1
            f.write(tplt.format(count, i[0], i[1], i[2], chr(12288)) + '\n')
                   
def main():
    start_url='https://book.douban.com/top250'
    movie_list = []    
    for i in range(10):
        html = get_page(start_url + '?start=' + str(25*i))
        parse_html(html, movie_list)
    write_file(movie_list)    

if __name__ == '__main__':
    main()
```

## 结果如下
![爬取结果](https://img-blog.csdnimg.cn/201907182103328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjM1ODA3Nw==,size_16,color_FFFFFF,t_70)



博主也是刚刚学习爬虫，囿于时间和水平有限，如有纰漏，欢迎指正！