---
layout: post
title: Python Crawling 7 네이버 실시간 검색어 및 블로그 검색
categories: Python
---

### 네이버 실시간 검색어

```python
import requests
from bs4 import BeautifulSoup

html = requests.get('https://www.naver.com').text
soup = BeautifulSoup(html, 'html.parser')

tag_list = soup.select('.PM_CL_realtimeKeyword_rolling .ah_item .ah_k')
for idx, tag in enumerate(tag_list, 1):
    print(idx, tag.text)
```



### 네이버 블로그 검색

```python
import requests
from bs4 import BeautifulSoup
from collections import OrderedDict
from itertools import count

def naver_blog_search(q, max_page=None):
    post_dict = OrderedDict()
    
    for page in count(1):
        url = 'https://search.naver.com/search.naver'
        params = {
            'query' : a,
            'where' : 'post',
            'start' : (page-1)*10 + 1,            
        }
        response = requests.get(url, params=params)
        html = response.text
        soup = BeautifulSoup(html, 'html.parser')
        
        print(response.request.url)
        
        for tag in soup.select('.sh_blog_title'):
            if tag['href'] in post_dict:
                return post_dict
            post_dict[tag['href']] = tag.text
            
        if max_page and (page >= max_page):
            return post_dict
```



### 연습문제

블로그 검색에서 타이틀+링크 외에 날짜, 요약설명을 추가로 크롤링



Source:  AskDjango/이진석 Python VOD