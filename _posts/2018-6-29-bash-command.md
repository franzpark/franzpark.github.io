---
layout: post
title: bash 명령어
categories: Dev
---

### bash 단축키

- Tab: 자동완성
- Ctrl + C: 중단하기(SIGINT)
- Ctrl + L: 화면 지우기 (리눅스 clear와 동일)
- Ctrl + T: 앞두글자 맞바꾸기
- Ctrl + R: 이전 명령어 검색
- Ctrl + X, Ctrl + V: bash 버전 표시
- Ctrl + X, Ctrl + X: 이전 커서 위치(토글)
- Ctrl + Z: 일시정지




- 바로 전 명령어 실행

  `!!`

- 부모 디렉토리로 가기 (스페이싱)

  `cd ..`

- 홈 디렉토리로 가기

  `cd`

- 터미널 끝내기

  `exit`

- 로컬 IP주소와 netmask 보기

  `ipconfig`

- 현재 디렉토리의 숨겨지지 않은 파일과 서브폴더 목록 보기. -r은 recursive이고 -a는 숨긴 파일을 포함한다.

  `ls`

- 현재 디렉토리의 모든 파일의 파일 접근 권한 보기. 권한의 포맷은 drwxrwxrwx이고 순서는 owner-group-other, 숫자값은 read=4, write=2, execute=1

  `ls -l filename`

- 새 디렉토리 만들기

  `mkdir dirname`

- 특정 디렉토리에 파일 옮기기

  `mv fild dir`

- file1을 file2로 이름바꾸기

  `mv file1 file2`

- 사용자가 현재 실행하고 있는 프로세스 목록 보기. 유용한 옵션 많으니 ps -help 사용.

  `ps -Af`

- 작업 디렉토리 보기

  `pwd`

- 파일 지우기

  `rm filename`

- 디렉토리와 디렉토리의 모든 컨텐츠 지우기

  `rm -rf dir`

- 현재 디렉토리의 txt로 끝나는 모든 파일 지우기

  `rm *.txt`

- 디렉토리 지우기 (비어있지 않을 때에만 동작)

  `rmdir dir`

- 원격 컴퓨터에 로그인하기

  `ssh IP address`

- 루트 쉘을 열고 exit 할 때까지 수퍼유저 권한을 갖는다. sudo su와 달리 사용자 환경변수에 상관없이 루트 쉘을 시작한다.

  `sudo -i`

- sudo -i와 같이 루트 쉘을 연다. 그러나 이 방법은 사용자 환경변수를 유지한다. exit로 일반쉘로 돌아간다.

  `sudo su`

- 특정 디렉토리와 그 안의 모든 파일의 압축 파일을 만들기

  `tar czf dirname.tgz dirname`

- 현재 디렉토리에 압축 파일을 풀기

  `tar zxvf archive`

- 빈 파일 만들기. 단, 파일이 없어야 한다.

  `touch filename`

- 컴퓨터에 로그인한 사용자 보기

  `who`

- 내 로그인 이름 보기

  `whoami`

- 파일 내용 보기

  `cat filename`

- 파일 찾기

  `find / -name filename`

- 파일 상태 보기

  `stat filename.txt`

- 인터넷에서 파일 받기

  `wget http://remote_file_url`

  
