---
layout: post
title: Python Crawling 2 HTTP(S) 요청 및 응답
categories: Python
---

### 크롤링을 하고자 한다.

- 크롤링할 대상들은 웹페이지 상에 존재한다.
- 웹페이지는 HTTP(S) 프로토콜을 통한 요청에 대한 **응답**
- 웹페이지는 기본적으로 HTML포맷이며, HTML문서 내에 이미지, 자바 스크립트, CSS 코드가 같이 기술됩니다.
  - 이미지, 자바스크립트, CSS 코드들은 브라우저에 의해서 처리되며, 그래픽적으로도 해석되어 유저에게 보여집니다.



### HTTP 요청을 하는 다양한 방법

- 웹브라우저 접속
- 새로고침
- 자바스크립트를 통한 요청 (ex: Ajax)
- 앱 API: 앱 http 클라이언트를 통한 요청
- HTML Form 전송



### HTTP 프로토콜

**Hypertext Transfer Protocol**

- 어떤 포맷의 데이터라도 전송할 수 있게 설계
  - HTML, CSS, JavaScript, Image, Video 등
- URI(Uniform Relources Identirifes) 를 통해 자원의 위치 지정
  - https://nomade.kr/vod/
    - https: 프로토콜 (http 혹은 https)
    - nomade.kr: 서버 도메인
    - /vod/: 요청할 자원의 이름
  - 현재 서버 및 다른 서버의 자원도 지정 가능

응답을 받은 브라우저를 HTML을 처음부터 1줄씩 해석해가며, 그래픽으로 보여준다.

네트워크 접근이 필요한 부분 (CSS/JavaScript/Image/Video URL) 을 만나면, 이에 대해 그 즉시 추가 HTTP 요청



### HTTP 메서드

**패킷을 어떻게 구성하느냐의 차이**

| Method                          | 설명                                                |
| ------------------------------- | --------------------------------------------------- |
| **GET** (크롤링에서 주로 사용)  | 리소스 요청                                         |
| **POST** (크롤링에서 주로 사용) | 대개 리소스 추가 요청이나 수정/삭제 목적으로도 사용 |
| PUT                             | 대개 리소스 수정 요청                               |
| DELETE                          | 대개 리소스 삭제 요청                               |
| HEAD                            | HTTP 헤더 정보만 요청. 해당 자원 존재여부 확인 목적 |
| OPTIONS                         | 웹서버가 지원하는 메서드 종류 반환 요청             |
| TRACE                           | 클라이언트의 요청을 그대로 반환                     |



### Get/POST 만을 살펴보자.

- GET 요청: "엽서"에 비유.
  - 엽서는 주소에 함께 메세지를 남긴다.
  - 물건을 보낼 수는 없다. (프로토콜 설계상 파일 업로드 불가)
  - 잘 설계된 서비스에서는 주로 **조회** 요청 시에 사용
- POST 요청: "소포"에 비유
  - 소포는 박스 안에 메세지나 물건을 같이 보낼 수 있다.
  - 파일 업로드 지원
  - 잘 설계된 서비스에서는 주로 **추가/수정/삭제** 요청 시에 사용



 ### HTTP 요청/응답 패킷 형식

- 요청 패킷
  - 요청헤더: 클라이언트에서 필요한 헤더 Key/Value 를 세팅한 후, 요청 전달
  - 첫번째 빈 줄: 헤더와 Body 구분자
  - Body: 클라이언트에서 필요한 Body를 세팅한 후, 요청 전달
- 응답 패킷
  - 응답헤더: 서버에서 필요한 헤더 Key/Value를 세팅한 후, 응답 전달
  - 첫번째 빈 줄: 헤더와 Body 구분자
  - Body: 서버에서 필요한 Body를 세팅한 후, 요청 전달



### 헤더란?

- HTTP 요청/응답 시에 헤더정보가 Key/Value 형식으로 세팅된다.
- 대개의 브라우저에서는 다음 헤더를 설정한다.
  - User-Agent: 브라우저 종류
  - Referer: 이전 페이지 URL (어떤 페이지를 거쳐서 왔는가?)
  - Accept-Language: 어떤 언어의 응답을 원하는가?
  - Authorisation: 인증정보
- 크롤링 시에는 User-Agent헤더와 Referer를 커스텀하게 설정할 필요가 있다.
  - 서비스에 따라 User-Agnet헤더와 Referer헤더를 통해, 응답을 거부하기도 한다. (ex: 네이버웹툰)



### Body란?

- HTTP 요청 시에는 Body가 없다. 응답에만 있다.
- 예시: HTML 코드, 이미지 데이터, JavaScript코드, CSS코드, 비디오 데이터 등



### HTTP 요청이 이뤄질 때의 요청/응답 패킷

**샘플코드**

```python
import requests

# GET요청 즉시 응답받음
response = requests.get('https://askdjango.github.io/lv1/')
html = response.text

print(html)
```



### 요청패킷 예시 / GET 요청 / 1) 헤더

**https://askdjango.github.io/lv1/**

```
GET / lv1/ HTTP/1.1
Host: askdjango.github.io
Connection: keep-alive
Accpet: */*
User-Agent: python-requests/2.14.2
Accept-Encoding: gzip, deflate
(빈줄)
(Body없음)
```



### 웹 요청/응답 처리 시에는 이와 같은 프로토콜의 문자열을 서로 주고 받아야 한다.

이를 쉽게 도와주는 것이 웹 클라이언트 라이브러리

- requests
  - 첫 응답만 받음. 추가 요청이 없음.
  - 단순한 요청에 최적화.
  - HTML응답을 받더라도, 이에 명시된 이미지/CSS/JavaScript 추가 다운을 수행 x => 직접 다운로드 요청은 가능
- selenium: 웹브라우저 자동화 툴
  - javascrpit/css 지원, 기존 GUI 브라우저 자동화 라이브러리
  - 사람이 웹서핑하는 것과 동일한 환경, 대신에 리소스를 많이 먹음
  - 웹브라우저에서 HTML에 명시된 이미지/CSS/JavaScript를 모두 자동 다운로드/적용



### 적은 리소스/빠른 처리를 위해, 가능하면 requests를 쓰라.

HTML 응답을 받을 경우

- requests는 서버에 추가요청을 하지 않는다. => 적은 리소스, 빠른 처리
- selenium은 기존 브라우저를 활용하기 때문에, 최소한 CSS 1개, JavaScript 2개, 이미지 1개에 대해 추가로 요청을 한다.



### selenium?

로그인 처리가 필요한가?

javascript 처리가 필요한가?

브라우저에 보면 데이터가 있는데, requests로 응답을 받아보면 데이터가 없나?

이도 저도 고민하기 싫다면, selenium을 쓰면 된다.

단 리소스를 많이 먹고, requests보다 처리 로직이 복잡해질 수 있다.



### 실전 코드

**커스텀 헤더를 설정하여, Http GET 요청을 해보자.**

```python
import os
import requests

# URL 소스: https://comic.naver.com/webtoon/detail.nhn?titleId=597478&no=223&weekday=mon
# 평범한 8반. 4장 30화. 오월(7)

image_urls = [
    'https://image-comic.pstatic.net/webtoon/597478/223/20180702010442_daad98c49c03d079d1e78b9b4c40109d_IMAG01_1.jpg',
    'https://image-comic.pstatic.net/webtoon/597478/223/20180702010442_daad98c49c03d079d1e78b9b4c40109d_IMAG01_2.jpg',
    'https://image-comic.pstatic.net/webtoon/597478/223/20180702010442_daad98c49c03d079d1e78b9b4c40109d_IMAG01_3.jpg'
]

for image_url in image_urls:
    headers = {
        'Referer' : 'https://comic.naver.com/webtoon/detail.nhn?titleId=597478&no=223&weekday=mon',
    }
    response = requests.get(image_url, headers=headers)
    image_data = response.content
    filename = os.path.basename(image_url)
    with open(filename, 'wb') as f:
        print('writing to {} {} bytes)'.format(filename, len(image_data)))
        f.write(image_data)
```



Source:  AskDjango/이진석 Python VOD