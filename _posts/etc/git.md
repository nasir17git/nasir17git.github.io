---
layout: single
title:  "Git 사용법 정리"
categories: 'etc'
---

이 블로그는 Github을 사용해 만들어졌고 관리된다.
다른 도구보다 깃헙을 사용한 깃블로그로 만든 이유는 커밋하는거랑 브랜치나뉘어서 버전 관리하고 통합하는것을 연습하기 위해서이다.
그래서 Git에 관한 내용을 정리하였다.


## Github 핵심 기능

1. 버전 관리
    commit하고 push할때마다 기록이 남아 업데이트 상황 파악이 용이함
2. 백업
    주중엔 PC에서 작업을 하고 주말에는 노트북으로 작업을 하는데, 레포지토리 받아오고 업로드하면서 동기화 가능
3. 협업
    나중에 팀프로젝트 같은 작업 진행 시 여러사람이 작업한 내용을 한꺼번에 동기화 가능

## 작업 흐름도

![flowchart](/assets/images/git.png)



## 초기 설정 진행

로컬 작업자의 이름, Email을 설정할 것이다

```
git config --global user.name "name"             > 이름 설정
git config --global user.email "root@domain.com" > 이메일 설정
git config --global user.name                    > 설정된 이름 보기
git config --global user.email                   > 설정된 이메일 보기
git config --global --list                       > 설정 상태 조회
```
- \-\-global 옵션을 제거할 시 현재 Repository만 대상으로 작업함.


## 개인용 Git 사용법

https://donghak-dev.tistory.com/219

https://mungto.tistory.com/187

1. .git 디렉토리 생성하기
`git init` 명령어로 버전관리를 할 git dir.을 생성함

2. 관리할 대상 파일/폴더 지정하기
```
git add filename    > 대상 파일 선택
git add .           > 전체 파일 대상 선택
```


## 협업용 Github 사용법