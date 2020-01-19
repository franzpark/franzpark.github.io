---
layout: posts
title: Python Lecture 4 블록문, 들여쓰기, 주석
categories: Python
---

**블록문 (Block Statement)**

블록문: 연속된 코드의 묶음

코드는 다수의 블록문으로 구성. 블록문 안에 다수의 블록문 중첩

블록 구분: 들여쓰기 (Indent), 다른 언어에서는 중괄호({})


**들여쓰기 (Indentation)**

들여쓰기는 Tabs 또는 Spaces로 입력

파이썬 언어 특징 중에서 가장 호불호가 갈린다

코드의 가독성 증대

하나의 들여쓰기는, 공백 4칸을 권장

**IndentationError**

Tab 과 Space는 엄연히 다른 글자. 하나로 통일하라

**탭 > 스페이스, 자동변환 기능**

Visual Studio Code 등

**주석 (Comments)**

소스코드를 설명하기 위한 목적

주석부분 실행되지 않는다.

파이썬에서의 주석 문법은 "1줄 주석" 문법만 지원

"여러 줄 주석" 문법은 "문자열"로서 표기

```python
# 주석1
# 주석2
```

문자열은 주석이 아니다.

**PEP 8: Style Guide for Python Guide**

PEP: Python Enhancements Proposals

일관성있는 코드 스타일을 위한 코드 스타일 제안

주요 스타일: 들여쓰기는 공백 4칸 (구글은 공백 2칸)

파이썬 코딩 컨벤션 by spoqa



Source:  AskDjango/이진석 Python VOD