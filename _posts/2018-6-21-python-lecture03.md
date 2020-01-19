---
layout: posts
title: Python Lecture 3 기본 자료 구조
date: 2018-6-21
categories: Python
---

### 기본 자료구조 (list, tuple, set, dict)

**Container**

여러 원소들을 가지고 있는 자료구조

list, set, dict, tuple

in 연산자로 멤버쉽 테스트를 지원

'>>> 'hello' in 'hello world'


**list**

생성문법: [], list(), list(iterable)

여러값을 순차적으로 저장, 순서를 보장

리스트를 한 줄로 쓸 때에는 대개 끝에 쉼표를 쓰지 않으며,

여러 줄로 나눠쓸 때에는 끝에 쉼표를 쓴다. 항목 추가/삭제 용이

```python
numbers = [1,3,5,7,9]
names = [
'Tom',
'Steve'
'Min',
]
```

색인 (index) 지원: 0부터 시작하여 1씩 증가

음수 색인 지원: 끝에서부터 역으로 -1 부터 1씩 감소

한 List 에 서로 다른 데이터타입의 값을 넣을 수 있지만, 가급적 같은 타입으로

```python
numbers = [1,3,5,7]		# 리스트 선언
print(7 in numbers)		# 멤버쉽 체크
print(len(numbers))		# 색인 0과 -3의 값 출력
for i in numbers:		# 리스트의 길이 출력
  print(i)				# for 루프를 통해 List 내 모든 값을 순회

bad_values = [10, 'Tom', (1,2,3)]	# BAD
```

범위 밖의 색인 (Index)을 참조하는 경우, IndexError 예외가 발생

'>>> numbers = [1,3,5,7]'

'>>> print(numbers[10])'

**데이터 변경**

```python
numbers = [1,3,5,7,5]
numbers[0] = 10			# 지정 색인의 값을 변경
numbers.append(9)		# 끝에 값을 추가
numbers.pop(3)			# 특정 색인값을 가져옴과 동시에 제거
numbers.remove(5)		# 특정 값을 1회 제거
numbers.insert(1,11)	# 특정 위치에 값을 추가
```

**값을 잘라내기 (Slice)**

```python
>>> numbers = [1,2,3,4,5,6,7,8,9,10]
>>> print(numbers[1:])
>>> print(numbers[1:7])
>>> print(numbers[1:7:2])
>>> print(numbers[::-1])
```

**리스트 합치기**

```python
>>> numbers1 = [1,3,5,7]
>>> numbers2 = [2,4,6,8]
>>> print(numbers1 + numbers2)	# 합쳐진 새로운 리스트
```

**List Comprehension**

```python
>>> numbers1 = [1,3,5,7]
>>> numbers2 = [2,4,6,8]
print([i + j for (i, j) in zip(numbers1, numbers2)])
```

**tuple**

생성문법: (), tuple(), tuple(iterable)

list와 유사하지만 변경 불가능(Immutable)한 특성

```python
>>> numbers = (1,3,5,7,9)
>>> numbers[0] = 10
>>> numbers.append(9)
```
소괄호는 때에 따라, 우선순위 연산자 혹은 튜플로 쓴다.

설정값으로서 튜플을 쓰실때, 헷갈리시면 리스트를 쓰자. 

성능 차이 거의 없다. 

코드 오류가 나지 않는 것이 중요.

```python
>>> tuple1 = (1+3)		# 우선순위
>>> tuple2 = (1+3,)		# 튜플
>>> tuple3 = (3)		# 우선순위
>>> tuple4 = (3,)		# 튜플

>>> list1 = [1+3]		#리스트
>>> list2 = [1,3,]		#리스트
>>> list3 = [3]			#리스트
>>> list4 = [3,]		#리스트
```

- Packing/Unpacking: list/tuple에 동일하게 적용
- unpacking 시에 갯수가 맞지 않으면 ValueError
- swap: list/tuple에 동일하게 적용

**set (집합형)**

- 중복을 허용하지 않는 데이터의 집합
- list/tuple에서 중복을 제거코자 할 때, set을 활용하면 유용
- List/tuple과 다르게 추가된 순서를 유지하지 않음.

```python
>>> set_numbers = {1,3,4,5,1,4,3,1}
>>> set_numbers
>>> list_numbers = [1,1,2,1,1,2,2,3,1,2,3]
>>> list_numbers = list(set(list_numbers))
>>> list_numbers
```

**합집합, 교집합, 차집합, 여집합 연산 지원**

```python
>>>set_numbers1 = {1,3,4,5,1,4,3,1,}
>>>set_numbers2 = {1,3,4,11,13,14,15,11,14,13,11}
>>>
>>>print(set_numbers1 - set_numbers2)	#차
>>>
>>>print(set_numbers1 - set_numbers2)	#합
>>>
>>>print(set_numbers1 - set_numbers2)	#교
>>>
>>>print(set_numbers1 - set_numbers2)	#여
```

**dict (사전형)**

Key 와 Value 의 쌍으로 구성된 집합

Key 중복을 허용하지 않음

중괄호 내에 콜론 (:) 으로 Key/Value를 구분

'dict_value = {'blue':10, 'yellow':3, 'blue':9, 'red':7}'

in 연산자로 멤버쉽 체크 지원 (Key의 등록 여부)
순회 시에는 key 목록만 순회
멤버함수
- .keys() :  key 목록
- .values() : value 목록
- .items() : (key, value) 목록



Source:  AskDjango/이진석 Python VOD