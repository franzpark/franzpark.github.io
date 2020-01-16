---
layout: post
title: JavaScript Lecture 2 숫자와 문자
categories: JavaScript
---

### 숫자

큰따옴표나 작은따옴표가 붙지않은 숫자는 숫자로 인식한다.

```javascript
alert(1+1);
2

alert(1.2 + 1.3);
2.5
```

곱하기를 할 때는 *(asterisk)를, 나누기를 할때는 /(slash)를 사용한다.

```javascript
alert(2 * 5);
10

alert(6 / 2);
3

Math.pow(3, 2);		// 9, 3의 2승
Math.round(10.6);	// 11, 10.6을 반올림
Math.ceil(10.2);	// 11, 10.2를 올림
Math.floor(10.6);	// 10, 10.6을 내림
Math.sqrt(9);		// 3, 3의 제곱근
Math.random();		// 0부터 1.0 사이의 랜덤한 숫자
```



### 문자

문자는 "(큰따옴표) 혹은 '(작은 따옴표) 중의 하나로 감싸야한다. 큰 따옴표로 시작하면 큰 따옴표로 끝나야하고, 작은 따옴표로 시작하면 작은 따옴표로 끝나야 한다. String 이라고 한다.

