---
layout: posts
title: Python Work-Automation 1 이메일 보내기
categories: Python
---

### 메세지를 보내는 다양한 방법

**이메일: 다양한 포맷의 메세지를 보낼 수 있음.**

- 대량 발송 시에는 대량 발송 서비스를 이용
- 일반 네이버메일/지메일 계정을 통해 대량 발송 시, 스팸우려해 발송거부 가능성
- 주로 쇼핑몰에서 활용
- SMTP(Simple Mail Transfer Protocol) 나 특정 서비스의 API를 활용



**문자 - SMS/LMS/MMS**

- 가장 손쉽게 보낼 수 있으나, 건당 발송비용이 발생
- 선거/마트 홍보 문자 등
- 그런데 요즘에는 사람들이 문자를 잘 안 본다
- 서비스 업체의 open api를 활용
- SMS, LMS, MMS 별로 가격 상이



**Android/iOS 앱 푸쉬**

- 메세지 발송에는 추가 비용이 없음.
- 단 푸쉬를 너무 많이 보내면, 고객들이 푸쉬를 끄거나, 앱을 지워버릴 수도
- Android/iOS 별로 발송방식이 상이 => 물론 일원화해서 관리토록 도와주는 서비스도 있음.



**메세지 서비스 활용: 카카오톡, 라인, 슬랙, 텔레그램 등**

- 서비스 업체의 OpenAPI를 통해 발송 가능. 대개 Free Quota 제공



### 이메일 발송 서비스 업체

- 개인 메일 서비스: 대량 메일 발송 시에 메일발송 거부 우려
  - SMTP 프로토콜을 통한 메일발송 지원
- 대량 메일 서비스: Amazon SES, SendGrid
  - SMTP 프로토콜 혹은 API를 통한 메일발송 지원



### 네이버 메일 SMTP를 통해 이메일을 발송해보자.

- 네이버 메일 접속
  - POP3/SMTP 사용 활성화
- SMTP 접속정보 확인
  - 서버명: smtp.naver.com
  - SMTP 포트: 465, 보안연결(SSL)
  - 필요아이디: 각자 네이버 아이디
  - 비밀번호: 각자 네이버 로그인 비밀번호



### 텍스트 메일 보내기

```python
import smtplib
from email.message import EmailMessage

import getpass
password = getpass.getpass('Password : ')

message = EmailMessage()
message['Subject'] = 'Francis 파이썬 업무자동화 - 이메일 텍스트'
message['From'] = 'mb146@naver.com'		# 송신자 이메일
message['To'] = 'franzpark@gmail.com, francis.park@outlook.com'	# 수신자 이메일 다수 (구분자: 콤마)
message.set_content('''이메일내용
안녕하세요. franzpark입니다.

이 부분에는 이메일의 내용을 쓸 수 있으며, HTML은 불가
HTML을 쓰면 태그가 그대로 노출됨.

좋은 인연이길 바랍니다.
감사합니다.''')

with smtplib.SMTP_SSL('smtp.naver.com', 465) as server:
    server.ehlo()
    server.login('mb146', password)
    server.send_message(message)
    
print('이메일을 발송했습니다.')
```



### HTML 메일 보내기

```python
message = EmailMessage()

# plain text 메세지
message.set_content(contents)

# html 메세지
message.add_alternative('''
    <h1>Francis 입니다</h1>
    
    <ul>
        <li>Romantic</li>
        <li>Nostalgia</li>
    </ul>
''', subtype='html')

#나머지 생략
```



### 파일 첨부하기

```python
import os
from email.mime.application import MIMEApplication

message = EmailMessage()

# 중략

# 이미지 첨부
filepath = './f1.jpg'
with open(filepath, 'rb') as f:
    filename = os.path.basename(filepath)
    img_data = f.read()
    part = MIMEApplication(img_data, name=filename)
    message.attach(part)
    
# 나머지 생략
```



### 이메일 내용으로 이미지 넣기

**첨부파일에 Content-ID를 지정하고, 이를 HTML내 img태그에서 src로 지정**

```python
import os
from email.mime.application import MIMEApplication

message = EmailMessage()

# 중략

message.add_alternative('''
    <h1>Francis 입니다</h1>
    
    <ul>
        <li>Romantic</li>
        <li>Nostalgia</li>
    </ul>
    
    <img src="cid:f1.jpg" />
''', subtype='html')

# 이미지 첨부
filepath_list = ['./f1.jpg', './f2.jpg', './f3.jpg']
for filepath in filepath_list:
    with open(filepath, 'rb') as f:
        filename = os.path.basename(filepath)
        cid = filename
        img_data = f.read()
        part = MIMEApplication(img_data, name=filename)
        part.add_header('Content-ID', '<' + cid + '>')
        message.attach(part)

# 나머지 생략
```



### 연습문제

**신작 네이버 웹툰 목록을 크롤링, 요약메일 발송해보기**

- 필요한 팩키지: requests, beautifulsoup4

```python
import os
import requests
from bs4 import BeautifulSoup

list_url = 'http://comic.naver.com/webtoon/weekday.nhn'
html = requests.get(list_url).text
soup = BeautifulSoup(html, 'html.parser')

comic_list = []

for a_tag in soup.select('a[href*=list.nhn]'):
    if not a_tag.select('.ico_updt'):
        continue
        
    img_tag = a_tag.find('img')
    title = img_tag.attrs['title']
    img_src = img_tag['src']
    img_name = os.path.basename(img_src)
    img_data = requests.get(img_src, headers={'Referer': list_url}).content
    
    comic_list.append({
        'title' : title,
        'img' : {
            'src' : img_src,
            'name' : img_name,
            'data' : img_data,
        },
    }) 
```

