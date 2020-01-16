---
layout: post
title: Python Crawling 10 네이버 카페
categories: Python
---

### TIP

- 크롤링 대상이 모바일페이지를 제공해주면, 모바일페이지를 크롤링하라.
  - http://cafe.naver.com: 데스크탑 페이지
  - http://m.cafe.naver.com: 모바일 페이지



### MechanicalSoup

- 쿠키처리, redirect처리 등 지원
  - requests.get를 통한 요청은 매 요청이 독립적으로 요청됨(requests.Session 사용 필요)
- 설치: pip3 install mechanicalsoup
  - 내부적으로 requests/beautifulsoup4를 사용



```Python
import mechanicalsoup

browser = mechanicalsoup.StatefulBrowser()
browser.open('URL')	# 방문할 주소

browser.select_form('form#form_id')
browser['id'] = 'id string'		# form의 name을 지정하여, 값을 할당
browser['pw'] = 'pw string'		# form submit

page = browser.get_current_page()
page.soup					# BeautifulSoup
```



### 네이버 로그인

- 네이버에 requests를 통해 Username/Password 로그인을 시도하면, 보안에 취약한 방식으로 로그인을 시도했다며, 로그인 거부
  - 이를 회피할 방법 검토 중
- 그럼 Selenium을 통해 웹브라우저 자동화를 해야하느냐?
  - NO!!!
  - **일회용 로그인 번호**를 통해 로그인을 하면 로그인이 가능하다.



Source:  AskDjango/이진석 Python VOD