---
layout: post
title: Python Work-Automation 3 CSV/엑셀 파일 만들기
categories: Python
---

### CSV

- 각 줄은 개행문자로 구분하며, 각 컬럼은 콤마(,)로 구분
- 스프레드시트/데이터베이스 소프트웨어에서 지원
- TSV(Tab-Separated Values) 포맷도 CSV라 퉁쳐서 부름
- Plain Text 파일이므로 다양한 인코딩으로 작성이 될 수 있다.
  - 한글 엑셀에서는 CSV파일을 생성하면 CP949 인코딩으로 생성. 그렇지만 UTF8 인코딩의 CSV파일을 읽어들일 순 있다.
  - 대개의 소프트웨어에서는 UTF8 인코딩으로 작성. 파이썬의 CSV모듈에서도 UTF8이 디폴트



### 왜 CSV파일을 쓰는가?

- 표 형식의 데이터를 Plain Text로 쉽게 생성이 가능
  - 파일크기가 작다.
- 거의 대부분의 프로그래밍 언어나, 데이터분석 프로그램에서 CSV파일을 지원 (Pandas, 엑셀, Github 등)
- 서식 지정없이 데이터만 생성코자 할 때 유용
- 파이썬 내장 라이브러리만으로 읽기/쓰기가 가능



### CSV 파일만들기 (라이브러리 사용)

```python
import csv

rows = [
    ['월요웹툰', '화요웹툰','수요웹툰'],
    ['신의 탑', '마음의 소리', '고수']
    ['귀전구담', '노블레스', '퍼미스미션'],
]

with open('webtoon.csv', 'wt', encoding='utf8') as f:
    writer = csv.writer(f)
    writer.writerows(rows)
    
with open('webtoon.csv', 'rt', encoding='utf8') as f:
    print(f.read())
```



### 구분자도 변경할 수 있다.

```python
import csv

rows = [
    ['월요웹툰', '화요웹툰','수요웹툰'],
    ['신의 탑', '마음의 소리', '고수']
    ['귀전구담', '노블레스', '퍼미스미션'],
]

with open('webtoon.csv', 'wt', encoding='utf8') as f:
    writer = csv.writer(f, delimiter='|')
    writer.writerows(rows)
```



### Tip: open 함수에 encoding을 지정해준 이유는?

encoding옵션을 지정하지않으면,

locale.getpreferredencoding(False) 값을 활용

```python
>>> import locale
>>> locale.getpreferredencoding(False)
'UTF-8'
```

시스템에 따라 변경되지 않고, UTF8 인코딩으로 생성할 것임을 명시



### CSV writer

**각 Row의 데이터가 list/tuple 일 때**

```python
import csv

writer = csv.writer(파일객체)

# 1 Row를 쓸 때
writer.writerow(['컬럼1', '컬럼2', '컬럼3'])

# 다수 Row를 쓸 때
writer.writerows([
    ['1행1열', '1행2열', '1행3열']
    ['2행1열', '2행2열', '2행3열']
])
```



### CSV writer

**각 Row의 데이터가 dict일 떄**

```python
import csv

fieldnames = ['first_name', 'last_name']
writer = csv.DictWriter(파일객체, fieldnames=fieldnames)

writer.writeheader()

# 1 Row를 쓸 때
writer.writerow({'first_name': 'Baked', 'last_name': 'Beans'})

# 다수 Row를 쓸 때
writer.writerows([
    {'first_name': 'Lovely', 'last_name': 'Spam'},
    {'first_name': 'Wonderful', 'last_name': 'Spam'}
])
```



### 이렇게?

```python
with open('webtoon.csv', 'rt', encoding='utf8') as f:
    for line in f:
        row = line.split(',')
        print(row)	# list type
        
# No !!!
```



### CSV 라이브러리를 쓰자.

```python
import csv

with open('webtoon.csv', 'rt', encoding='utf8') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)	# list type
```



### dict으로 받을려면

```python
import csv

with open('webtoon.csv', 'rt', encoding='utf8') as f:
    fieldnames = None	# 디폴트. None이면 첫번째 Row가 fieldnames으로 지정
    reader = csv.DictReader(f, fieldnames=fieldnames)
    for row in reader:
        print(row)	# dict type
```



### Tip: 생성한 csv파일을 윈도우 엑셀에서 열고자 할 때

**인코딩 이슈가 발생할 수 있다. 한글이 깨진 것처럼 보인다.

상황: 데이터 분석팀에서는 엑셀로 데이터분석을 한다.

- CSV 파일은 Plain Text 파일로서 인코딩을 명시할 수 없다.
- 한글 윈도우의 기본 인코딩은 CP949이며, 엑셀도 그러하다.
  - 하지만 윈도우에서는 BOM를 통해 인코딩을 명시할 수 있다.



### CSV 인코딩별 처리

| 구분           | CP949     | UTF8 (추천) | UTF8 BOM     |
| -------------- | --------- | ----------- | ------------ |
| 엑셀           | OK        | 옵션 지정   | OK           |
| G 스프레드시트 | OK        | OK          | OK           |
| Pandas         | 옵션 지정 | OK          | OK           |
| Python         | 옵션 지정 | OK          | BOM 제거필요 |



### 코드

```python
# Pandas에서 CP949인코딩의 CSV읽기
df = pandas.read_csv('cp949.csv', encoding='cp949')	# DataFrame

# Python에서 CP949인코딩의 CSV읽기
with open('cp949.csv', encoding='cp949') as f:
    reader = csv.reader(f)
    
# Python에서 UTF8 BOM ㅣㅇㄺ기
import csv
import codecs

with open('웹툰_utf8_with_bom.csv', 'rb') as f:
    content = f.read()
    if content.startswith(codecs.BOM_UTF8):
        bom_size = len(codecs.BOM_UTF8)
        content = content[bom_size:]
    string= content.decode('utf8')
    
    reader = csv.reader(string.splitlines())
    for line in reader:
        print(line)
```



### 다양한 엑셀 지원 라이브러리

- **tablib**: Python Module for Tabular Datasets in XLS, CSV, JSON, YAML, C
  - csv, json, yaml, xls(x) 포맷에 대한 import/export 지원
- openpyxl: A Python library to read/write Excel 2010 xlsx/xlsm files
- pyexcel: Python Wrapper that provides one API for reading, manipulating and writing data in csv, ods, xls, xlsx and xlsm files
- xlwt, xlrd



### tablib

```python
rows = [
    ['월요웹툰', '화요웹툰','수요웹툰'],
    ['신의 탑', '마음의 소리', '고수']
    ['귀전구담', '노블레스', '퍼미스미션'],
]

import tablib
data = tablib.Dataset()
data.headers = rows[0]
for row in rows[1:]:
    data.append(row)
    
    data.json	# 문자열 반환
    data.yaml	# 문자열 반환
    data.xlsx	# xlsx 바이너리 반환
    
with open('webtoon.xlsx', 'wb') as f:
    f.write(data.xlsx)
```



### 연습문제

<네이버 웹툰의 요일별 전체웹툰> 목록을 csv파일로 생성해보라.

```python
import requests
from bs4 import BeautifulSoup

html = requests.get('http://comic.naver.com/webtoon/weekday.nhn').text
soup = BeautifulSoup(html, 'html.parser')

# 나머지를 구현해보자.
# 크롤링부분은 크롤링 VOD 참고
```

