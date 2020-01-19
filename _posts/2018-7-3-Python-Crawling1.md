---
layout: posts
title: Python Crawling 1 웹크롤링 시작하기
categories: Python
---

Crawling == Web Scrapping

### Crawing

"Web scraping is a computer software technique of extracting information from websites."

- 사람이 웹페이지에 접속해서 정보를 읽어들이듯이
- 인터넷 상에 흩어져있는 자료들을 사람 대신에 프로그램을 통해 서핑하며 수집&가공
- 이때 프로그램 구성에 따라 서핑능력의 차이가 발생.
  - 대표적으로 자바스크립트 처리 여부



### 크롤링으로 해볼 수 있는 것

- **Bot 프로그램**과 연계하기 참 좋다
- 오늘 날씨정보를 읽어와서, 메세지 알림
- 공연예매 빈 좌석이 생기면, 메세지 알림 혹은 자동 예매
- 중단 예정인 서비스가 있는 데, 자료백업을 지원하지 않는다면, 직접 백업
- 학교 식당 메뉴를 찾아보기 힘들다면, 식사 1시간 전에 오늘의 메뉴 메세지 알림
- 가사가 들어있지 않은 MP3파일에 자동 가사 기입
- 국회의원 입법현황을 읽어와서 => 분석(Pandas 등)

주의: 정보를 읽어올 수 있다고 해서, 마음대로 활용할 수 있다는 것은 아니다. 저작권 유의



### 다양한 웹페이지 구현

**똑같아 보이는 웹페이지라 할지라도 구현방법이 모두 다를 수 있다.**

- 필요한 컨텐츠가 HTML을 통해 표현이 모두 되어있을 경우 - **가장 쉽고 빠른 처리**
- 필요한 컨텐츠가 서버로부터 JSON 응답을 받아서 처리할 경우
- 필요한 컨텐츠가 JavaScripts를 통해 처리할 경우
- 인증이 필요한 웹페이지
- 다양한 웹 프론트엔드 프레임워크를 통해 표현되는 페이지
- ActiveX가 있는 페이지

이외에도 정말 다양한 방식으로 구협한 웹페이지가 있다. 그 웹페이지에 맞게 적절하게 크롤링 루틴을 적용해다 한다.



###  World Wide Web

- 프로토콜: HTTP, HTTPS

  - **H**yper **T**ext **T**ransfer **P**rptocol

- 최근 서비스는 웹 기반의 서버/클라이언트 구조에서 시작

  - 웹프레임워크를 통한 **생산성 극대화**

- 서비스에 따라 차후 socket 기반의 서버 구조로 변경하기도 한다.

  - ex) 카카오톡"겁나빠른황소" 프로젝트



### 다양한 HTTP(S) 클라이언트

- GUI 브라우저: Chrome, Firefox, Sarafi, Internet Explorer, Opera 등
- CLI 브라우저: w3m, elinks, lynx 등
- CLI 브라우저 (단순요청): curl, wget 등
- **파이썬 라이브러리/프레임워크**
  - requests, selenium, Scrapy



### HTTP(S)는 선 클라이언트 요청, 후 서버 응답

- 클라이언트가 먼저 요청하면, 서버는 이에 응답
  - 대게 웹페이지 접속, 새로고침, 링크 클릭을 통한 페이지 전환
  - Ajax 요청, 앱 API 요청
- 클라이언트 요청없이, 서버에서 먼저 클라이언트로 데이터 전송 불가
  - 물론 웹소켓이라는 기술이 있긴 합니다만, 이것도 클라이언트 측에서 먼저 서버로 연결을 하고 있어야 합니다.
- 서버: 클라이언트 요청을 처리하고, 그에 대한 응답 생성 (장고/플라스크/nginx/아파치 웹서버)
  - 다양한 응답 포맷: **html**, css, javascripts, jpg 등
- 클라이언트: 서버로부터의 응답을 받아서, 로직에 따라 처리
  - 브라우저: 서버로부터 HTML응답을 받아, 이를 해석하여 그래픽으로 표현. 필요하다면 서버로 javascripts, css, jpg 등을 추가 요청/다운로드. 자바스크립트를 통해 서버로 Ajax 요청을 전달
  - Android/iOS 앱: 내부에 웹브라우저(WebView)를 내장하기도 하고, 네이티브 코드로 앱 API를 호출하여 대개 JSON응답을 받아서 처리
- 계속 반복



### 웹페이지 샘플

**모두 같은 모습을 한 웹페이지이지만, 구현이 모두 다르다.**

- [레벨1] 단순 HTML 크롤링
- [레벨2] Ajax 렌더링 크롤링
- [레벨3] 자바스크립트 렌더링 크롤링



### [레벨1] 단순 HTML 크롤링

```python
import requests
from bs4 import BeautifulSoup

html = requests.get('https://franzpark.github.io/lv1/').text
soup = BeautifulSoup(html, 'html.parser')

for tag in soup.select('#course_list > .course > a')
    print(tag.text, tag['href'])
```



### [레벨2] Ajax 렌더링 크롤링

```python
import requests

course_list = requests.get('https://franzpark.github.io/lv2/data.json').json()
for course in course_list:
    print(course['name'], course['url'])
```



### [레벨3] 자바스크립트 렌더링 크롤링

```python
import json
import re
import requests

html = requests.get('https://franzpark.github.io/lv3/').text

# 중략

for course in course_list:
    print(course['name'], course['url'])
```



Source:  AskDjango/이진석 Python VOD