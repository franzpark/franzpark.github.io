---
layout: post
title: Python Algorithm Quiz 1
categories: Python
---



## 문제01 1부터 n까지의 합 구하기

1. 1부터 10까지의 수를 모두 더하면?

```python
j = 0
for i in range(1, 11):
    print (i, end=" ")
    j += i
print ("sum =", j)
```

2. 1부터 100까지의 수를 모두 더하면?

```python
j = 0
for i in range(1, 101):
    print (i, end=" ")
    j += i
print ("sum =", j)
```

3. 1부터 n까지 연속한 숫자의 제곱의 합을 구하는 프로그램을 for 반복문으로 만들어 보라

   (예를 들어 n = 10이라면 1^2^ + 2^2^ + 3^2^ + ... + 10^2^ = 385를 계산하는 프로그램)

```python
j = 0
for i in range(1, 11):
    print (i, end=" ")
    j += i**2
print ("sum of square of each number =", j)
```



## 문제02 최댓값 찾기

4. 주어진 숫자 n개중 가장 큰 숫자를 찾는 알고리즘을 만들어라.

   이번 문제는 주어진 숫자 n개 중에서 가장 큰 숫자(최댓값)을 찾는 문제. 예를 들어 17, 92, 18, 33, 58, 7, 33, 42와 같이 숫자가 여덟개가 있을 때 최댓값은 92

   최댓값 찾기 알고리즘을 살펴보기 전에 여러 숫자를 효율적으로 다루는 데 꼭 필요한 파이썬의 리스트 기능을 정리해 보자.

   앞으로 여러가지 알고리즘을 접하다 보면 최댓값 또는 최솟값 자체를 구해야 할 때도 있지만, 최댓값 또는 최솟값의 위치 번호를 알아야 할 때도 많다. 물론 최댓값의 위치 번호를 알면 최댓값도 쉽게 구할 수 있다.

   - a[최댓값의 위치 번호] = 최댓값
   - 예시: a[1] = 92

   ```python
   # 최대값과 최대값의 위치찾기
   
   a = [17, 92, 18, 33, 58, 101, 7, 33, 42]
   
   def maxx(b):
       ret = 0
       rett_location = 1
       for i in b:
           if ret < i:
               ret = i
               dd = rett_location
           rett_location += 1
       print ("maximum number is ", ret)
       print ("The location of maximum number is ", dd, "번째")
       
   maxx(a)
   maximum number is  101
   The location of maximum number is  6 번째
   ```

   ```python
   # 최소값과 최소값의 위치찾기
   
   a = [17, 92, 18, 33, 58, 7, 33, 42]
   
   def minn(b):
       ret = b[0]
       ret_location = 0
       for i in b:
           if ret >= i:
               ret = i
               cc = ret_location
           ret_location += 1
       print ("minimum number is ", ret)
       print ("The location of minimum number is ", cc)
       
   minn(a)
   minimum number is  7
   The location of minimum number is  6    
   ```

   ``` python
   # 최소값/위치찾기 by Sedric
   number = [17, 92, 19, 22, 33, 5, 42, 1]
   first = number[0]
   for i in range(len(number)):
       if first >= number[i]:
           first = number[i]
   print("최소 값은 {}이며, 인덱스는 {}입니다".format(first, number.index(first)))
   ```

   