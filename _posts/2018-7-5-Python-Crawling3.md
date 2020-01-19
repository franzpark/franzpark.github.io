---
layout: posts
title: Python Crawling 3 requests 라이브러리 살펴보기
categories: Python
---

### Requests: HTTP for Humans

- 파이썬에서는 기본 라이브러리로  urllib이 제공되지만, 이보다 간결한 코드로 다양한 HTTP 요청을 할 수 있는 최고의 라이브러리
- JavaScript 처리가 필요한 경우에는 selenium을 고려할 순 있다. 하지만 이 경우에도 requests 적용이 가능할 수도 있다. 크롤링할 페이지에 대해 다각도로 검토가 필요하다.

크롤링 시에 웹요청에 requests를 쓸 수 있다면, 가장 효율적인 처리



### 단순 GET 요청

```python
import requests

response = requests.get('http://news.naver.com/main/home.nhn')
html = response.text

from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'html.parser')
for tag in soup.select('a[href*=sectionList.nhn]'):
    print(tag.text.strip())
```



### GET 요청 시에 커스텀헤더 지정

```python
request_headers = {
    'User-Agent' : ('Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36) (KHTML, like Gecko) chrome/58.0.3029.110 Safari/537.36'),
    'Referer' : 'http://news.naver.com/main/home.nhn', # 뉴스홈
}

response = requests.get('http://news.naver.com/main/home.nhn')
```

requests 라이브러리에서의 기본 User-Agent 값은 'python-requests/버전'이다. 서버에 따라 User-Agent값으로 응답 거부여부를 결정하기도 한다.



### GET 요청 시에 GET인자 지정

- params 인자로 dict지정: 동일 Key의 인자를 다수 지정 불가

```python
get_params = {'mode' : 'LSD', 'mid' : 'shm', 'sid1' : '105'}	#IT/과학 탭을 위한 GET 인자
response = requests.get('http://news.naver.com/main/home.nhn', params=get_params)
```

- params 인자로 (key, value) 형식의 tuple 지정: 동일 Key의 인자를 다수 지정 가능

```python
get_params = [('mode', 'LSD'), ('mid', 'shm'), ('sid1', '105')]
response = requests.get('http://news.naver.com/main/home.nhn', params=get_params)

get_params = (('k1', 'v1'), ('k1', 'v3'), ('k2', 'v2'))
response = requests.get('http://httpbin.org/get', params=get_params)
```



### 상태코드

```python
>>> response.status_code	# int
200

>>> response.ok	# status_code가 200이상 400미만의 값인지 여부 (bool)
True
```



### 응답헤더

- dict 타입이 아니라 requests.structures.CaseInsensitiveDict 타입
  - Key문자열의 대소문자를 가리지 않는다.
- 각 헤더의 값은 헤더이름을 Key로 접근하여 획득

```python
>>> response.headers
>>> response.headers['Content-Type']	# Key문자열 대소문자에 상관없이 접근
'text/html; charset=UTF-8'
>>> response.headers['content-type']
'text/html; charset=UTF-8'
>>> response.encoding
'UTF-8'
```

response.encoding값은 Content-Type헤더의 charset값으로 획득. Content-Type헤더에 charset값이 없을 경우 iso-8859-1로 처리될 수 있다.



### 응답 body

```python
bytes_data = response.content	# 응답 Raw 데이터 (bytes)
str_data = response.text		# response.encoding으로 디코딩하여 유니코드 변환
```

- 이미지 데이터일 경우에는 .content만 사용

```python
with open('flower.jpg', 'wb') as f:
    f.write(reponse.content)
```

- 문자열 데이터일 경우에는 .text를 사용

```python
html = response.text
html = response.content.decode('utf-8')	# 혹은 .content 필드를 직접 디코딩
```

- json 포맷의 응답일 경우
  - json.loads(응답문자열)을 통해 직접 Deserialise를 수행
  - 혹은 .json()함수를 통해 Deserialise 수행
  - 응답문자열이 json포맷이 아닐 경우 JSONDecodeError 예외 발생

```python
import json

obj = json.loads(response.text)
obj = response.json()	# 위와 동일
```



### 한글이 깨진 것처럼 보여질 경우

*.charset 정보가 없을 경우, 먼저 utf8로 디코딩을 시도하고 UnicodeDecodeError가 발생할 경우, iso-8859-1 (latin-1)로 디코딩을 수행. 이때 한글이 깨진 것처럼 보인다.*



이때는 다음과 같이 직접 인코딩을 지정한 후에, .text에 접근하자.

```python
>>> response.encoding
'iso-8859-1'
>>> response.encoding = 'euc-kr'
>>> html = response.text

혹은 .content를 직접 디코딩할 수도 있다.
>>> html = response.content.decode('euckr')
```



### 단순 POST 요청

response = requests.post('http://httpbin.org/post')



### POST요청 시에 커스텀헤더, GET인자 지정

```python
request_headers = {
    'User-Agent' : ('Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36) (KHTML, like Gecko) chrome/58.0.3029.110 Safari/537.36'),
    'Referer' : 'http://httpbin.org',
}
get_params = {'k1' : 'v1', 'k2' : 'v2'}

response = requests.get('http://httpbin.org/post',
                        headers=request_headers,
                        params=get_params)
```



### 일반적인 Form 전송/요청

게시판에 글쓰기

- data인자로 dict지정: 동일 Key의 인자를 다수 지정 불가

```python
data = {'k1' : 'v1', 'k2' : 'v2'}
response = requests.post('http://httpbin.org/post', data=data)
```

- data인자로 (key, value) 형식의 tuple 지정: 동일 Key의 인자를 다수 지정 가능

```python
data = (('k1', 'v1'), ('k1', 'v3'), ('k2', 'v2'))
response = requests.post('http://httpbin.org/post', data=data)
```



### JSON POST 요청

JSON API 호출시

```python
# JSON 인코딩
import json
json_data = {'k1': 'v2', 'k2': [1, 2, 3], 'name': 'franzpark'}

# json포맷 문자열로 변환한 후, data인자로 지정
json_string = json.dumps(json_data)
response = requests.post('http://httpbin.org/post', data=json_string)

# 객체를 json인자로 지정하면, 내부적으로 json.dumps 처리
response = requests.post('http://httpbin.org/post', json=json_data)
```



### 파일 업로드 요청

```python
# multipart/form-data 인코딩
files = {
    'photo1': open('f1.jpg', 'rb'),		# 데이터만 전송
    'photo2': open('f2.jpg', 'rb'),
    'photo3': ('f3.jpg', open('f3.jpg', 'rb'), 'image/jpeg', {'Expires': '0'}),
}
post_params = {'k1': 'v1'}
response = requests.post('http://httpbin.org/post', files=files, data=post_params)
```



Source:  AskDjango/이진석 Python VOD