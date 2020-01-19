---
layout: posts
title: bash 에서 alias 추가
categories: Dev
---

**bash shell에서 alias 추가**

1. cd ~ 를 이용해서 홈폴더로 이동한다.
2. vi .bashrc 로 .bashrc 파일을 생성한다.
3. alias [커맨드]=[파일경로] 라인을 생성한다.

```
Francis

1. alias typ = 'C:\Program Files\Typora\Typora.exe' 를 생성
2. error 발생
3. alias typ="C:/'Program Files'/Typora/Typora.exe" 로 수정
4. error corrected
```

