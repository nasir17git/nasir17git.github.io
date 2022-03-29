---
title:  'W3 리눅스 어드민'
categories: 'review'
---

3주차에 진행된 Linux Admin 과정의 종합 정리.

Admin과정이 뭔가했는데 서버 운영 관련 주요 시스템 설정을 관리하는 법을 의미하는 듯 하다.

**주요 키워드**

- 사용자 및 그룹 관리, 권한 관리(ACL), 
- 작업예약(at/crond)
- 디스크 마운트 및 RAID
- systemd와 daemon, 
- RPM 및 YUM 패키지 관리
- Network Manager 및 nmcli,  

----

### 사용자 및 그룹 관리

|생성|변경|삭제|
|---|---|---|
|useradd\groupadd|usermod\groupmod|userdel|groupdel|

유저 비밀번호를 설정할때, -p 옵션 대신 passwd 명령어로 설정
그냥-p로 할시 암호화가 되어있지않아 로그인 불가

> /etc/passwd 유저 정보가 저장됨.
유저명:암호:UID:GID:Comment:홈디렉토리:사용SHELL
user1:x:1000:1000:usercommenttest:/home/user1:/bin/bash

> /etc/group 그룹 정보가 저장됨.
그룹명:암호:GID
user1:x:1000

> /etc/shadow 유저 비밀번호 정보가 저장됨.
> /etc/gshadow 그룹 비밀번호 정보가 저장됨.

### 권한설정 

**리눅스 파일, 디렉토리 권한 확인**

`ls - l` 

파일종류,권한,링크수,사용자,그룹,파일크기.수정시간,파일이름
-rwxr-xr-x 1 encore encore 5720 Feb 21 14:32 testdir

- \- 파일(-),디렉토리(d)
- rwx 사용자 권한
- r-x 그룹 권한


chmod 권한명 경로/파일명