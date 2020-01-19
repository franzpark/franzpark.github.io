---
layout: posts
title: Python Lecture 5 흐름제어
categories: Python
---

**조건문, if문**

if 조건:

elif 두번째조건:

elif 세번째조건:

elif 네번째 조건:

else:

한 if statement 에서 if / else 는 1회만 쓸 수 있다.
그러나 elif는 원하는 조건의 수만큼 쓸 수 있다.

**반복문: for**

Inerable Object 로부터 가져올 값들을 모두 가져올 때까지 반복

for 변수 in 리스트:

	매 항목마다 수행할 코드 블록

for 내에서 break 문을 만나면 해당 반복문 종료

```python
for i in range(20):
    print(i)
    if i > 10:
        break
```

**중첩 반복문**

반복문 내에서의 break는 **#근접한 반복문**만 종료시킵니다.

```python
>>> for i in range(2, 10):
        for j in range(1, 10):
            print(i, j)
            break
```

**중첩 반복문을 한 번에 종료시키기**

예외 발생이나 함수 내에서 return 문을 통해 중첩 반복문을 한 번에 종료 가능

```python
def gugu():
    for i in range(2, 10):
        for j in range(1, 10):
            print(i, j)
            return None
```

**내장함수 range**

- range(stop): 0부터 stop 미만의 범위에서 1씩 증가시킨 값으로 리스트를 구성
   - 문법적으로는 리스트가 아니라, 순회가능한(Iterable) 객체
- range(start, stop[, step]): start값 이상, stop값 미만의 범위에서 step 씩 증가시킨 값으로 리스트를 구성

**반복문 while**

조건이 만족하는 동안에 반복문 수행

while 조건:
    매 항목마다 수행할 코드 블록
    
while 내에서 break문을 만나면 해당 반복문 종료

```python
i = 0
while i < 20:
    print(i)
    if i > 10:
        break
    i += 1
```

**무한루프**

반복문 조건 이 항시 True일 때

```python
i = 10
while i < 13:
    print(i)
    i -= 1
```

for 에서는 itertools.count 함수를 통해 가능

```python
from itertools import count

for i in count(1):
    print(i)
```

**Assignment: 구구단 (2,4,6,8단)**

```python
i = 1
numbers2 = [0]
numbers4 = [0]
numbers6 = [0]
numbers8 = [0]
while i<10:
    numbers2.append(2*i)
    numbers4.append(4*i)
    numbers6.append(6*i)
    numbers8.append(8*i)
    i += 1
print(numbers2[1:])
print(numbers4[1:])
print(numbers6[1:])
print(numbers8[1:])
```

**구구단 (2,4,6,8단) by Sedric**

```python
for i in range(2, 9, 2):
    for j in range(1, 10):
        print('{} x {} = {}'.format(i, j, i*j))
```

**구구단 (2,4,6,8단) using List Compr. by Sedric**

```python
[i*j for i in range(2, 9, 2) for j in range(1, 10)]
```



Source:  AskDjango/이진석 Python VOD