---
layout: post
title: "【Python】批量提取指定网站上的特定信息"
author: "Yingwei Guo"
categories: 
tags: 爬虫
image: 
---



## 介绍

> 理论上，凡是**重复且有规律**的事情都可以用编程来解决，编程的优势也在于可以大大降低我们在繁琐事情上所耗费的时间和精力。

![在这里插入图片描述](https://img-blog.csdnimg.cn/24010210922f432588e8a379cddb01d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Ya35pyI44CB5peg5aOw,size_20,color_FFFFFF,t_70,g_se,x_16)

例如上图的网页，其中包含的表格有四千多页，并且没有提供所有表格信息的下载按钮，只能一页一页去复制，这时候，我们用爬虫就可以非常简单地解决这个问题：利用requests库获取网页内容，利用beautifulsoup库解析和获取网页中我们需要的信息，最后将其输入到一个文件中，针对多个网页只需要循环上述步骤即可。
## 脚本内容

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
'''
Author: Guo Yingwei
Date: 2021-10-09 16:40:07
E-mail: willgyw@126.com
Description:  
LastEditors: gyw
LastEditTime: 2021-10-09 20:57:34
'''
import requests
from bs4 import BeautifulSoup
import requests.packages.urllib3
#import codecs
import re

def get_page(url):
    try:
        kv = {'user-agent':'Mozilla/5.0'}
        requests.packages.urllib3.disable_warnings()
        r = requests.get(url,headers = kv, verify=False)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return '错误'

def parse_html(html):
    soup = BeautifulSoup(html, 'html.parser')    
    gene_list = soup.find('table', attrs={'class': 'table table-bordered table-striped'}).find('tbody').find_all('tr')
    return_lines_lst = []
    for gene in gene_list:
        name = gene.find('th').find('a').find('i').get_text()
        td_lst = gene.find_all('td')
        info_lst = []
        for td in td_lst:
            try:
                info = re.search('(?<=">).*?(?=</td>)', str(td)).group()
                if info == '':
                    info = 'NAN'
            except:
                info = 'NAN'
            info_lst.append(info)
        return_lines = name + '\t' + '\t'.join(info_lst) + '\n'
        return_lines_lst.append(return_lines)
    return return_lines_lst

def write_file(return_lines_lst):
    with open('all_gene.txt','a+', encoding='utf-8') as fo:
        for return_lines in return_lines_lst:
            fo.write(return_lines)


def main():
    start_url = 'https://gdap.org.cn/allgene.php?page='
    for i in range(100):
        html = get_page(start_url + str(i+1))
        lines_lst = parse_html(html)
        #print(lines_lst)
        write_file(lines_lst)

if __name__ == '__main__':
    main()
```
