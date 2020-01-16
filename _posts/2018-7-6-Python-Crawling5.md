---
layout: post
title: Python Crawling 5 Pillow를 활용한 이미지 썸네일/다운로드 처리
categories: Python
---

### 크롤링할 대상

- HTML 문서 + JSON
- **이미지**
- PDF, EXCEL 등 여러 정적인 파일들



### 이미지

- 이미지를 다운받기
- 고화질의 이미지를 받더라도, 경우에 따라 작은 용량으로 줄일 필요가 있음.
  - 썸네일 처리
- 대개 여러 개의 파일로 나뉘어져 있음.
  - ex) 웹툰 - 하나의 파일일 경우, 로딩에 긴 시간이 소요
  - 이미지 합치기
- 이미지를 다른 포맷으로 변환하기 (jpg, png 등)



### 파이썬 이미지 라이브러리

- PIL: Python Image Library (마지막 업데이트: 2009년)
- Pillow: PIL Fork
- PILkit: PIL 유틸리티 콜렉션
- Wand: ImageMagick 파이썬 바인딩



### Pillow

- 설치: 쉘 > pip3 install pillow
- PIL 프로젝트 (마지막 릴리즈 2009년)의 대체 프로젝트: PIL 라이브러리와 완전 호환
- 활용 예: 이미지 썸네일 생성하기, 다수 이미지 합성하기, 다른 이미지 포맷으로 변환하기, 회전하기 등
- 장고에서는 models.ImageField 필드를 쓸 때, Pillow 설치 필수
- Python Imaging Library Handbook



### 웹에서 자주 쓰이는 이미지 포맷

- jpg: 주로 사진을 저장할 때
  - 이미지품질 옵션: 0(저) ~ 100(고)
- gif: 움직이는 이미지. 저품질.
- png: 투명지원되는 이미지 포맷



### Case 1) 이미지를 다운받기

**requests의 response.content는 응답원본: bytes 타입

```python
import os
from PIL import Image
import requests

image_url = ('https://www.fmkorea.com/files/attach/new/20180706/36'
             '55299/412183146/1141309037/b7e3d8c3b6fe4fc6a261fa2a91ace163.png')
image = requests.get(image_url).content		# 서버응답을 받아, 파일내용 획득
filename = os.path.basename(image_url)		# URL에서 파일명 획득
with open(filename, 'wb') as f:
    f.write(image)							# 파일에 저장

from IPython.display import Image
Image(filename='b7e3d8c3b6fe4fc6a261fa2a91ace163.png')
```



### Case 2) 이미지 품질 낮추기 / 다른 포맷으로 변경하기

```python
from IPython.display import Image

with Image.open('b7e3d8c3b6fe4fc6a261fa2a91ace163.png').convert("RGB") as im:
    im.save('sje_quality_80.jpg', quality=80)	# quality 옵션은 jpg에서만 유효
```



### Case 3) 이미지를 용량으로 줄이기

**이미지 가로/세로 크기 줄이기**

- resize(size, resample=0)
  - 리사이징된 "Image 복사본" 생성
  - 원본의 가로/세로 **비율 무시**, 지정 크기로 강제 리사이징
- **thumbnail**(size, resample=3)
  - 원본 "Image 객체"를 변경
  - 원본의 가로/세로 **비율 유지**하면서, 지정 크기로 리사이징

이미지는 크기를 줄이거나 늘리거나, 약간의 변경도 모두 **손실**

```python
from PIL import Image

# image thumbnail
with Image.open('sje_quality_80.jpg') as im:
    print('current size : {}'.format(im.size))
    im.thumbnail((300, 300))	# 원본을 변경
    im.save('python3_thumb.png')	# png format
```



### Case 4) 이미지 이어 붙이기

**여러 개로 나눠진 이미지들을 이어 붙이기

```python
from PIL import Image

WHITE = (255, 255, 255)

with Image.open('img1.jpg') as im1:
    with Image.open('img2.jpg') as im2:
        # 이미지 2개를 세로로 이어서 붙일려고 한다.
        width = max(im1.width, im2.width)
        height = sum((im1.height, im2.height)
        size = (width, height)
        
        with Image.new('RGB', size, WHITE) as canvas:
            canvas.paste(im1, box=(0, 0))			# left/top 지정
            canvas.paste(im2, box=(0, im1.height))	# left/top지정
            canvas.save('canvas.jpg')
            
투명채널(alpha)을 살리고 싶다면, alpha_composite
```



### 연습문제

**네이버웹툰. 다운받은 개별 파일들을 하나의 파일로 합치기**

```python
import os
import requests
from bs4 import BeautifulSoup

ep_url = 'https://comic.naver.com/webtoon/detail.nhn?titleId=670150&no=124&weekday=sat'
html = requests.get(ep_url).text
soup = BeautifulSoup(html, 'html.parser')

for tag in soup.select('.wt_viewer img'):
    img_url = tag['src']
    img_name = os.path.basename(img_url)
    headers = {'Referer': ep_url}
    img_data = requests.get(img_url, headers=headers).content
    
    with open(img_name, 'wb') as f:
        f.write(img_data)
        
WHITE = (255, 255, 255)
with PILImage.open('20180629213657_9828e67f93a41146100d1f988d872c69_IMAG01_1.jpg') as im1:
    with PILImage.open('20180629213657_9828e67f93a41146100d1f988d872c69_IMAG01_2.jpg') as im2:
        width = max(im1.width, im2.width)
        height = sum((im1.height, im2.height))
        size = (width, height)
        
        with PILImage.new('RGB', size, WHITE) as canvas:
            canvas.paste(im1, box=(0, 0))
            canvas.paste(im2, box=(0, im1.height))
            canvas.save('img_merged.jpg')
```



Source:  AskDjango/이진석 Python VOD