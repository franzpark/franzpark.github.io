---
layout: posts
title: Python Lecture 2 데이터타입
date: 2018-6-21
categories: Python
---

### 기본 데이터 타입 (int, float, str)

변수 (Variables) = 임시로 저장하는 공간

```python
name = "Hello, Python"
birth = 1990
age = 2017 - birth
print(name, age)
```

**데이터 타입 (Data Types)**
변수는 하나의 데이터를 담아두는 공간. 그릇개념

효율성. 유한한 자원.

**Numeric Type (숫자)**
정수형: int

실수형: float

사칙연산: (+,-,*,/), 몫(//), 나머지(%), 지수 승 (*) 연산자

수의 법위 제한이 없음.

**Boolean Type (참, 거짓)**
- 참은 True, 거짓은 False
- 비교 연산자의 결과는 Boolean Type
  - <, <=, >, <=, ==, !=
  - is, is not: 참조 비교
- 논리 연산자
  - or, and, not

```python
>>> a = []
>>> b = []
>>> id(a), id(b)
>>> a == b
>>> a is b
```

**다른 타입에서의 Boolean 판단**
숫자 0은 False, 그 이외에는 True

빈 문자열은 False, 그 이외에는 True

빈 list/tuple/set.dict은 False, 그 이외에는 True

```python
>>> bool(0), bool(1), bool(-1)
>>> bool(''), bool(' '), bool('a')
>>> bool([]), bool(()), bool({}), bool(set()), bool([' '])
```

**String Type (문자열)**
문자열을 홑따옴표 (')로 감싸거나, 쌍따옴표 (")로 감싸기

name1 = 'Python'

name2 = "Python"


홑(쌍)따옴표 1개로 감싼 문자열 안에 홑(쌍)따옴표를 문자열로서 처리하고자 할 경우, 해당 홑(쌍)따옴표를 ESCAPE 처리

name = 'I\'m Tom'

파이썬은 여러 줄 문자열 문법을 지원합니다. 홑(쌍)따옴표 3개로 감싸주는 문법

**함수 위치 인자 (Positional Arguments)**

```python
'{0}, {1}, {2}'.format('a', 'b', 'c')
'{}, {}, {}'.format('a', 'b', 'c')
'{2}, {1}, {0}'.format('a', 'b', 'c')
'{2}, {1}, {0}'.format(*'abc')    # unpacking argument sequence
'{0], {1}, {0}'.format('a', 'b')  # arguments' indices can be repeated
```

**함수 키워드 인자 (Keyword Arguments)**

```python
'Coordinates: {lat}, {lng}'.format(lat='37.24N', lng='-115.81W')
coord = {'lat': '37.24N', 'lng': '-115.81W'}
'Coordinates: {lat}, {lng}'.format(**coord)
```

**NameError**

정의되지 않은 변수에 접근 시에 발생



Source:  AskDjango/이진석 Python VOD