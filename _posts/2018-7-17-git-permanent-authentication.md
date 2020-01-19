---
layout: posts
title: 터미널에서 git 사용시 자동 로그인
categories: Dev
---

Github 의 git repository를 clone 해서 개인컴퓨터에서 작업을 하고, push 나 pull을 할 때 마다 매번 아이디와 비번을 요구한다.



빈번하게 작업하는 경우, 아이디 / 비번 입력이 번거롭게 느껴지는 경우가 있는데, git repositories 계정과 패스워드를 설정해두고 자동으로 로그인하는 방법이 있다. (Permanent authentication)

```shell
$ git config credential.helper store
$ git push https://github.com/repo.git		# 본인의 repository

Username for 'https://github.com': <USERNANE>
Password for 'https://USERNAME@github.com': <PASSWORD>
```



일정 시간이 지나고, 저장해둔 자동로그인 정보를 해제시키고 싶다면, (caching expire)

```shell
$ git config --global credential.helper 'cache --timeout 7200'
```

위와 같이 입력하면, 7,200초 (= 2시간) 동안 로그인정보가 캐시에 저장이 된다.