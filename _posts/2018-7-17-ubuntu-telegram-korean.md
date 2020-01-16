---
layout: post
title: Ubuntu 18.04 LTS 텔레그램 한글입력 하는 법
categories: Linux
---

우분투 18.04 LTS 를 설치하면 기본 입력기가 iBus 로 설정이 되어있다.



iBus를 사용해도 대부분의 응용프로그램들 상에서 한글입력이 문제없이 되지만, 텔레그램상에서 한글이 입력되지 않는 문제가 있다.



텔레그램상에서 한글입력을 하려면 iBus 입력기를 fcitx 입력기로 바꾸어 줌으로써 해결할 수 있다.



Ubuntu Software로 들어가서 fcitx 를 인스톨 해주고, 터미널창에서 추가로 두 가지를 더 인스톨해야한다.



```shell	
$ sudo apt-get install fcitx-hangul
$ sudo apt-get install fcitx-config-gtk
```



설치를 마친 후 우측상단 키보드 모양 아이콘을 마우스 오른쪽클릭해서 설정으로 들어간다.

1. 입력방법 탭에서 하단의 '+' 를 눌러 Hangul을 추가 해준다.
2. 전역설정탭에서 입력기 전환 방법을 설정한다.



이제 텔레그램에서도 한글을 입력할 수 있다.