---
layout: post
title: Python Crawling 6 크롤링 예시 3가지
categories: Python
---

### Atom 셋팅

- Atom을 실행시킨후 Packages에서 아래의 것들을 인스톨한다.

  - autocomplete-python
  - script

- Python 스크립트 실행하는 단축키는 ctrl + shift + b

- 스크립트 실행시 'cp949'에러가 발생하면 ctrl+alt+shift+o 를 눌러서, environment variable에 아래를 추가해준다.

  - > PYTHONIOENCODING=utf-8 



UL 태그 = Unordered List

OL 태그 = Ordered List



### 레벨1 - 단순 HTML 크롤링

```python
import requests
from bs4 import BeautifulSoup

lv1_url = 'https://askdjango.github.io/lv1/'
html = requests.get(lv1_url).text
soup = BeautifulSoup(html, 'html.parser')

for a_tag in soup.select('#course_list .course a'):
    print(a_tag.text, a_tag['href'])
```



### 레벨2 - Ajax 렌더링 크롤링

```python
import json
import requests

lv2_data_json_url = 'https://askdjango.github.io/lv2/data.json'
json_string = requests.get(lv2_data_json_url).text
course_list = json.loads(json_string)

for course in course_list:
    # print(course['name'], course['url'])
    print('{name} {url}'.format(**course))
```



### 레벨3 - 자바스크립트 렌더링 크롤링

```python
import json
import re
import requests

lv3_url = 'https://askdjango.github.io/lv3/'
html = requests.get(lv3_url).text
matched = re.search(r'var courses = (.+?);', html, re.S)
json_string = matched.group(1)

course_list = json.loads(json_string)
for course in course_list:
    print('{name} {url}'.format(**course))
```

