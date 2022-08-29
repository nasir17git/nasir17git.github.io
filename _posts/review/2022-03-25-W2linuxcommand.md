---
title:  'W2 리눅스 커맨드'
categories: 'review'
---

# 리눅스 기초

## 1. 리눅스 기본 환경

### 1) 컴퓨터의 주요 구성 요소

- HW
  - CPU
  - Memory
  - Disk
  - NIC(Network Interface Card)
  - GPU
  - ...
  - **IO(Input/Output): 입출력 장치**
    - Input: 키보드
    - Output: 모니터

- SW  
  - Kernel
  - **Shell**(CLI)
    - sh
    - **bash**
    - zsh

### 2) CLI

- 명령: command
- 옵션: option
  - -o
  - --option
- 인자: argument
  ```
  command -option argument
  ```
  
  - 명령
  - 명령 옵션
  - 명령 인자
  - 명령 옵션 인자
  
  ```
  <command>; <command>
  ```
  
  ```
  ls \
  -l
  ```

### 3) Manual Page

```bash
man <command>
man -s <section> <command>
```

- space: 다음 페이지
- b: 이전 페이지
- enter: 다음 줄
- q: 종료
- /검색어
  - n: 내림
  - N: 올림

https://en.wikipedia.org/wiki/Man_page 참고해서 볼것

## 2. 디렉토리 및 파일 보기

pwd, ls, cd ,file

### 1) 디렉토리

#### (1) 현재 작업 디렉토리 확인

```
pwd
```

#### (2) 디렉토리 변경

```
cd <directory>
```

#### (3) 파일 목록

```
ls
```

- `-F`
  
  - `/`: 디렉토리
  - `*`: 실행 파일
  - `@`: 심볼릭 링크/소프트 링크
  - 없는: 텍스트 파일

- `-a`
  
  - .
  - ..

- `-l`

```
drwxr-xr-x. 2 playdata playdata 6 Feb 14 15:00 Desktop
```

- d: 파일의 형식
  
  - `d`: 디렉토리
  - `l`: 심볼릭 링크
  - `b`: 블록 장치
  - `c`: 캐릭터 장치(원시(raw) 장치)
  - `p`: 파이프(Pipe)
  - `s`: 소켓 파일
  - `-`: 텍스트 파일 / 실행 파일

- `rwxr-xr-x`: 퍼미션, 권한
  
  - `rwx`: 소유자 권한
  - `r-x`: 소유 그룹 권한
  - `r-x`: 기타 사용자 권한

- `.`: Access Control List(ACL): 추가 퍼미션, 권한

- `2`: 하드 링크 수

- `playdata`: 소유자(Owner)

- `playdata`: 소유 그룹(Group)
  
  - `id` 명령어

- `6`: 파일 크기(Byte)
  
  - `-h`: Human Readable

- `Feb 14 15:00`: Modified time
  
  - MAC Timestamp
    
    - M: Modified(파일 내용 변경)
    
    - A: Access
    
    - C: Changed(파일 속성 변경)
    
    ```
    stat <file>
    ```

- `-R`: 하위 디렉토리 및 파일 확인

- `-d`: 디렉토리 그 자체를 가리킴

```
file <파일명>
```

#### (5) 절대 경로 vs 상대 경로

- 절대 경로: PATH(경로) 표현 /(루트) 표현 방법
  - /home/playdata/pathtest/testfile

- 상대 경로: 현재 작업 디렉토리로 부터 표현 방법
  - ./pathtest/testfile

### 2) 파일(텍스트 파일)

cat, more, less, head, tail, wc

- cat
  
  - `-n`: 라인 번호

- wc
  
  - 라인 수
  - 워드 수
  - 문자 수

## 3. 디렉토리 및 파일 내용 변경

cp, mv, touch, mkdir, rm, rmdir, ln

### 1) 복사

```
cp [option] source(s) target
```

| source | target |
| ------ | ------ |
| 파일     | 파일     |
| 파일     | 디렉토리   |
| 디렉토리   | 디렉토리   |

- `-i`: 덮어쓸것인지 확인
- `-r`: 디렉토리 복사

### 2) 이동

```
mv [option] source(s) target
```

- `-i`: 덮어쓸것인지 확인

source와  target의 경로가 같으면 이름 변경

### 3) 생성

#### (1) 파일

```
touch <filename>
```

빈 파일 생성

이미 파일이 있다면 mtime을 현재 시각으로 변경

-> atime, ctime, mtime

#### (2) 디렉토리

```
mkdir <directory>
```

- `-p`: 부모 디렉토리도 생성

### 4) 삭제

#### (1) 디렉토리

```
rmdir <directory>
```

반드시 디렉토리에 아무 파일이 없어야 함

```
rm -r <directory>
```

#### (2) 파일

```
rm <file(s)>
```

- `-r`: 디렉토리 삭제

### 5) 링크

#### (1) 심볼릭(Symbolic)/소프트 링크

```
ln -s source target
```

디렉토리도 링크 생성

다른 장치에 생성 가능

#### (2) 하드 링크

```
ln source target
```

디렉토리는 생성 불가

다른 장치에 생성 불가

## 4. 파일 및 디렉토리 검색

grep, egrep, fgrep, find

### 1) 파일 내용 검색

#### (1) grep

Global Regular Expression Print

Regular Expression: RE, 정규식, 정규화 표현식

```
grep [option] <pattern> <file>
```

| 옵션  | 설명          |
| --- | ----------- |
| -n  | 라인 번호       |
| -i  | 대소문자 무시     |
| -w  | 단어          |
| -v  | 패턴 제외       |
| -l  | 파일명         |
| -R  | 재귀, 하위 디렉토리 |

- Meta Character, 메타 문자
  
  | 문자   | 의미                  |
  | ---- | ------------------- |
  | ^    | 시작                  |
  | $    | 끝                   |
  | .    | 한 문자                |
  | *    | 모든 문자, 문자 X, 앞 문자 X |
  | [ ]  | 한 문자 대치             |
  | [^ ] | [ ] 제외              |

ex: r*t = t, rt, rXt, rXXt, ... , rXXXXXXXXXt

ex: r.*t = rt, rXt, rXXt, ... , rXXXXXXXXXt

#### (2) egrep

Extended Global Regular Expression Print

메타문자: ?, |, { }, ( ), + 의미가 해석 됨

| 문자   | 설명     |
| ---- | ------ |
| x\|y | x 또는 y |
| ( )  | 그룹     |

ex:

```
egrep '(ba|z)sh' passwd
```

#### (3) fgrep

Fixed Global Regular Expression Print

메타 문자의 의미 해석을 하지 않음

### 2) 파일 검색

```
find <PATH> <OPTION> <ARGUMENT>
```

| 옵션     | 설명           |
| ------ | ------------ |
| -name  | 파일 이름        |
| -type  | 파일 형식        |
| -perm  | 권한           |
| -size  | 용량           |
| -atime | 접근 시간        |
| -user  | 소유자          |
| -ls    | 파일의 상세 정보 확인 |

- 파일 형식
  
  - `d`: 디렉토리
  - `l`: 심볼릭 링크
  - `b`: 블록 장치
  - `c`: 캐릭터 장치(원시(raw) 장치)
  - `p`: 파이프(Pipe)
  - `s`: 소켓 파일
  - `f`: 텍스트 파일 / 실행 파일

## 5. 텍스트 편집기

vi -> vim

## 6. 권한

Permission

### 1) 권한 확인

```
rwx r-x r-x
123 456 789
```

- 123: Owner(소유자)
- 456: Owner Group(소유 그룹)
- 789: Others(기타 사용자)
- r / -: Read / Deny: 4
- w / -: Write / Deny: 2
- x / - : Execute / Deny: 1

사용자 생성 - 사용자 이름과 같은 그룹 생성 여기에 포함    
-> `id`

파일

- 읽기? 

- 쓰기?
  
  > 실행 쓰기 권한 필요? X

- 실행?

        실행 파일만 필요

디렉토리

- 읽기?

- 쓰기?

- 실행?

r: 4, w: 2, x: 1, -: 0
**rwx: 7**
**rw-: 6**
**r-x: 5**
**r--: 4**
-wx: 3
-w-: 2
--x: 1

**---: 0**

### 2) 기본 권한

파일: 666 - 002 = 664

디렉토리: 777 - 002 = 775

umask: 002

umask:  기본 파일 생성 권한 제어

### 3) 권한 변경

#### (1) 심볼릭 모드

change mode

```
chmod mode filename
```

mode = who / operation / permission

- who:
  
  - u: user/owner
  - g: group/group owner
  - o: others
  - a: all(user+group+others)

- operation
  
  - +: 추가
  - -: 제거
  - =: 대치

- permissions
  
  - r
  - w
  - x

#### (2) 8진수(Octal) 모드

```
chmod mode filename
```

mode: 777, 765 ...

### 4) 특수 권한

실행 권한과 관련

- SetUID: setuid가 설정된 파일을 실행하는 모든 사용자는 이 파일의 소유자의 권한으로 실행된다.
  
  - u+xs
  - 4000

- SetGID: setgid가 설정된 파일을 실행하는 모든 사용자는 이 파일의 소유 그룹의 권한으로 실행된다.
  
  - g+xs
  - 2000

- Sticky Bit
  
  - o+xt
  - 1000

Permission: How to use?

Rights: What to do?

Subject --> Object

System Access Control

- DAC(Discretionary Access Control): 임의적 접근제어
- MAC(Mandatory Access Control): 강제적 접근제어
- Non-DAC -> RBAC(Role Based Access Control): 역할 기반의 접근 제어

## 7. 사용자 및 그룹

사용자: id, 계정(account), 로그인 사용자
그룹: 사용자 모아놓은 객체

### 1) 사용자 전환

root: 관리자, super user

> Least Privileges: 최소 권한
> 
> Need to known: 알 권리

su, sudo

```
sudo <command>
```

```
sudo -i
sudo -i -u <account>
```

/etc/sudoers

```
%wheel ALL=(ALL) ALL
```

- `%wheel`: wheel 그룹에 속한 사용자
- `ALL`: 모든 시스템에서
- `(ALL)`: 모든 사용자로
- `ALL`: 모든 명령어를

### 2) 사용자 관리

useradd, usermod, userdel

```
useradd <account>
```

```
userdel <account>
```

- `-r`: 홈디렉토리 함께 삭제

/etc/passwd

```
root:x:0:0:root:/root:/bin/bash
```

- root: 사용자 명
- x: 패스워드 -> placeholder
- 0: UserID(UID)
- 0: GroupID(GID) - 주 그룹 GID
- root: GECOS(General Electronics Comments) - 설명(실제 사용자 이름)
- /root: 홈디렉토리
- /bin/bash: 로그인 쉘

/etc/shadow

```
root:X:Y:0:99999:7:A:B:C
```

- root: 사용자 명

- X: 암호화된 패스워드
  
  - `$N`: 해쉬 알고리즘
    - `$salt`: 솔트 값
    - `$XXX`: 암호화된 패스워드
    
    > !!: Lock
    > 
    > *: Disable

- Y: 패스워드를 마지막으로 변경한 시각
- 0: 패스워드 최소 사용 기간
- 99999: 패스워드 최대 사용 기간
- A: 패스워드 변경 경고 기간
- B: 비활성화/유예기간 시간
- C: 만료일

```
passwd <account>
```

### 3) 그룹 관리

groupadd, groupmod, groupdel

주(Primary) 그룹: 사용자는 반드시 하나의 주 그룹에만 속함
보조(Secondary) 그룹:  사용자는 선택적으로 여러 보조 그룹에 속할 수 있음 

/etc/group

```
root:x:0:A
```

- root: 그룹 명
- x: 그룹 패스워드 -> place holder
- 0: GroupID(GID)
- A: 해당 그룹에 속한 사용자 목록(보조 그룹)

## 8. 쉘

### 1) 쉘 메타 문자

#### (1) 경로 관련 메타 문자

`~`, `-`, `~<account>`

#### (2) 파일 이름 관련 메타 문자

`*`, `?`, `[]`

#### (3) 인용부호 메타 문자

```
' '    모든문자 탈출
" "    $ \ ` " 만 의미 해석
` `    명령의 결과를 대
$( )
```

> `$`: 변수를 지칭

> Escape 탈출
> 
> `\`

#### (4) I/O 방향 재지정(redirection) 메타 문자

`>`, `>>`, `<`, `<<`, `|`

| fd  | 장치         | 약어     |
| --- | ---------- | ------ |
| 0   | 표준 입력(키보드) | STDIN  |
| 1   | 표준 출력(모니터) | STDOUT |
| 2   | 표준 에러(모니터) | STDERR |

fd: file descriptor(파일 설명자)

```
command < filename
command 0< filename
```

```
command > filename
commnad 1> filename
command 2> filename
command 1> filea 2> fileb
command 1> filex 2>&1
```

```
command >> filename
```

```
command1 | command2
```

command1 명령의 표준 출력 command2 명령의 표준 입력 제공

```
A | grep <pattern>
A | more
...
```

Here Documents, Here Text, Here String ... => heredoc

```
command <<(here-documents)
```

```
cat <<EOF > a.txt
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> EOF
```

```
cat <<EOF | python2
> print("hello here doc")
> EOF
```

```
cat <<EOF | wc -l
1
2
3
4
5
EOF
```

### 2) 쉘 히스토리

```
history
```

~/.bash_history

```
!<number>
```

### 3) 환경설정 파일

/etc/profile

/etc/profile.d/*

/etc/bashrc

/etc/bashrc.d/*

~/.profile

~/.bash_profile

~/.bashrc

...

환경 변수를 확인

```
export
env
```

환경 변수 -> 환경 설정 파일

- PATH: 명령을 찾을 경로
- HISTSIZE: 히스토리 저장 개수
- PS1: 프롬프트 모양

> https://bashrcgenerator.com/

1. 부팅
   
   - /etc/profile
   - /etc/profile.d/*
   - /etc/bashrc

2. 로그인
   
   - ~/.profile
   - ~/.bash_profile --> 로그인
   - ~/.bashrc --> 터미널을 실행

```
source <환경변수파일명>
```

### 4) Alias

```
alias
```

```
alias lf="ls -F"
unalias lf
```

## 9. 프로세스 관리

```
lscpu
```

- socket
- core
- thread

### 1) 프로세스

- Program (Disk)
- Process (Memory)
- Thread (Instruction)


```
ps
```
- -e: 모든 프로세스
- -f: 상세


#### (1) 유형

- Foreground
  
  - 터미널 대화식 명령 실행한 프로세스
    - 실행 --> 종료

- Background
  
  - 시스템 실행 비대화식 프로세스 -> 데몬(Daemon) -> 서비스(Service)
    - 계속적으로 실행되는



백그라운드로 실행

```
command &
```

#### (2) 상태

- 부모 프로세스
- 자식 프로세스
  - 고아 프로세스
  - 좀비 프로세스

exit status
exit code
return code
...
https://media.geeksforgeeks.org/wp-content/uploads/Wait_system_call_in_c.jpg

종료 코드 확인
```
echo $?
```
- 0: 정상 종료
- 양수: 비정상 종료

```
sleep 100 &
```

```
jobs
```

```
fg %<JobID>
```
Ctrl-c : 강제 종료
Ctrl-z : 중지

```
bg %<JobID>
```

#### (3) 시그널(Signal)
`kill` 명령은 시그널을 전송

```
kill -l
```

- 1 SIGHUP(Hangup): 
- 2 SIGINT(Interrupt): Ctrl-c
- 9 SIGKILL(Kill): 강제 종료(무시할수 없음)
- 15 SIGTERM(Terminate): 정상 종료

```
kill -N <PID>
```
> 기본: 15

`pkill` 명령
```
pkill -N <Pattern>
```

#### (4) 실시간 프로세스 확인
`top` 명령

- PID
- USER
- PR: Priority 우선순위
- NI: Nice (-20 ~ 19)
- VIRT: Virtual Memory(RAM + SWAP)
- RES: Residence Memory
- SHR: Shared Memory
- S: 상태
  - R: 실행 / 실행 가능
  - S: Idle 유휴
  - D: 인터럽트 없는 유휴 상태
  - T: 중지 또는 추적
  - Z: 좀비
- %CPU
- %MEM
- TIME+
- COMMAND

- SHIFT-t: CPU 사용 시간으로 정렬
- SHIFT-m: 메모리 사용율 기준 정렬
- SHIFT-p: CPU 사용율 기준 정렬
- k: kill
- u: user
- space bar: refresh
- m: memory info toggle
- t: cpu info toggle

```
uptime
```
load average: 0.38, 0.24, 0.16
                1m   5m    15m
부하 평균

lscpu
CPU: 1 = 1.00 = 100%
CPU: 4 = 4.00 = 100%

부하 생성
```
sha256sum /dev/zero
```

## 10. 아카이브 / 압축

### 1) 아카이브
`tar` 명령
Tape Archive

아카이브
```
tar -cf <archive.tar> <files>
```

내용 확인
```
tar -tf <archive.tar>
```

풀기
```
tar -xf <archive.tar>
```

- `-v`: Verbose 자세히


### 2) 압축

`gzip`, `bzip2`, `xz`

#### (1) gzip
gzip 압축
```
gzip <file>
```

gzip 압축 해제
```
gzip -d <file>
또는
gunzip <file>
```

#### (2) bzip2
bzip2 압축
```
bzip2 <file>
```

bzip2 압축 해제
```
bzip2 -d <file>
또는
bunzip2 <file>
```

#### (3) xz
xz 압축
```
xz <file>
```

xz 압축 해제
```
xz -d <file>
또는
unxz <file>
```

#### (4) tar와 통합
- `-z`: gzip
- `-j`: bzip2
- `-J`: xz
