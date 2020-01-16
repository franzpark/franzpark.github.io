---
layout: post
title: Python Crawling 8, 9 post 방식 페이지 분석
categories: Python
---

### 네이버 웹툰 목록 크롤링

```python
from collections import OrderedDict
from itertools import count
from urllib.parse import urljoin
import requests
from bs4 import BeautifulSoup

def get_list(title_id):
    list_url = 'http://comic.naver.com/webtoon/list.nhn'
    ep_dict = OrderedDict()
    
    for page in count(1):
        params = {'titleID': title_id, 'page': page}
        print('try {}'.format(params))
        
        list_html = requests.get(list_url, params=params).text
        soup = BeautifulSoup(list_html, 'html.parser')
        
        for tag in soup.select('.viewList tr td.title'):
            tag_a = tag.find('a')
            is_up = bool(tag.find('img'))
            link = urljoin(list_url, tag_a['href'])
            title = tag_a.text
            print(title, is_up, link, img_url)
            
            if link in ep_dict:
                return ep_dict
            
            ep = {
                'title': title,
                'is_up': is_up,
                'link': link,
                'img_url': img_url,
            }
            ep_dict[link] = ep
            
>>> get_list(650305)
```



### 특정 에피소드의 이미지 다운받기

```python
def ep_download(ep_url):
    html = requests.get(ep_url).text
    soup = BeautifulSoup(html, 'html.parser')
    
    comic_title = soup.select('.comicinfo h2')[0].text
    ep_title = soup.select('.tit_area h3')[0].text
    
    comic_title = ' '.join(comic_title.split())
    ep_title = ' '.join(ep_title.split())
    
    img_path_list = []
    
    for tag in soup.select('.wt_viewer img'):
        img_url = tag['src']
        headers = {'Referer': ep_url, 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'}
        
        img_name = os.path.basename(img_url)
        img_path = os.path.join(comic_title, ep_title, img_name)
        
        dir_path = os.path.dirname(img_path)
        if not os.path.exists(dir_path):
            os.makedirs(dir_path)
            
        print(img_url, end=' : ')
```



### 이미지를 세로로 합치기 (주요코드)

```python
from PIL import Image

im_list = []
for img_path in img_path_list:
    im = Image.open(img_path)
    im_list.append(im)
    
canvas_size = (
    max(im.width for im in im_list),
    min(65500, sum(im.height for im in im_list))
)
    
print(canvas_size)
    
canvas = Image.new('RGB', canvas_size)
    
top = 0
for im in im_list:
    canvas.paste(im, (0, top))
    top += im.height
    
canvas.save('canvas.jpg')
```



### Tip: 포맷별 최대 지원 크기

- jpg는 최대 2^16-1 (65,535) 필셀
- png는 최대 2^31-1 (2,147,483,647) 픽셀 (signed)



Source:  AskDjango/이진석 Python VOD