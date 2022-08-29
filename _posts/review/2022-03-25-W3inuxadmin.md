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

\> /etc/passwd 유저 정보가 저장됨.    
유저명:암호:UID:GID:Comment:홈디렉토리:사용SHELL     
user1:x:1000:1000:usercommenttest:/home/user1:/bin/bash    

\> /etc/group 그룹 정보가 저장됨.   
그룹명:암호:GID       
user1:x:1000   

\> /etc/shadow 유저 비밀번호 정보가 저장됨.   
\> /etc/gshadow 그룹 비밀번호 정보가 저장됨.   

### 권한설정 

**리눅스 파일, 디렉토리 권한 확인**

`ls - l`   

파일종류,권한,링크수,사용자,그룹,파일크기.수정시간,파일이름       
-rwxr-xr-x. 1 encore encore 5720 Feb 21 14:32 testdir   

- \- 파일(-),디렉토리(d)
- rwx 사용자 권한
- r-x 그룹 권한
- r-x 기타(other)권한
- . ACL 설정여부 . ACL 설정없음 / + ACL 설정있음

**권한 변경**

	chmod 권한명 경로/파일명

- 기호로 변경하는법  
	`chmod 카테고리,연산자기호,권한문자 대상 파일`

	|구분|문자|의미|
	|---|---|---|
	|카테고리|u/g/o/a|사용자/그룹/기타/전체|
	|연산자기호|+/-/=|권한부여/권한제거/권한설정|
	|권한문자|r/w/x|읽기권한/쓰기권한/실행권한|

- 숫자로 변경하는법
	rwx로 표시되는 권한은 2진수 3자리로 간주하여 8진수 숫자로 권한을 부여할 수 있다   
	`chmod 숫자 대상파일`

	|접근권한|숫자표시|
	|:---:|:---:|
	|rwxrwxrwx|777|
	|rwxr-xr-x|755|
	|rw-r--r--|644|

**특수권한**

- setuid,setgid,sticky bit
- 권한 문자앞에 부여하여 특수한 권한을 부여할 수 있음
	- setuid: 실행시에만 소유자 권한으로 실행; 권한을 빌려옴
	- setgid: 실행시에만 그룹 권한으로 실행; 권한을 빌려옴
	- sticky bit: 생성은 자유롭지만 변경 및 삭제 불가; 공유모드

	|구분|문자|의미|
	|---|---|---|
	|uid|4000\u+s|실행권한이 있으면 소문자s, 없으면 대문자S|
	|gid|2000\g+s|실행권한이 있으면 소문자s, 없으면 대문자S|
	|sticky bit|1000\u+t|실행권한이 있으면 소문자t, 없으면 대문자T|

- ACL
- Access Control List, 파일이나 디렉토리에 특정 사용자 또는 그룹의 권한 부여
	- getfacl 파일명또는디렉토리명 > ACL 설정 조회
	- setfacl -m u:유저명:권한 g:그룹명:권한 o::권한 파일명또는디렉토리명
	- -m > mask. 최소한의 권한, 특정사용자나 그룹에게 권한을 부여해도 effective 가 mask에 맞춰 줄어듬
	- setfacl -b 옵션으로 제거가능 

### 작업 예약

1. 단발성 작업 예약

	- at 명령어
	- 작업 저장 경로: /var/spool/at/

	```
	at [option] "시간"
	"시간" > 시간이나 yy-mm-dd
	-t 시간형식 > 자세한 입력방법은 man at 참고
	-f 파일명 > 파일 실행
	-l > 예약 작업 리스트 확인
	-c 작업번호 > 예약 작업 상세보기
	-d 작업번호 > 예약 작업 삭제
	```



2. 반복적 작업 예약

	- crontab 명령어
	- 작업 저장 경로: /var/spool/cron
	- 실행 로그 확인: cat /var/log/cron

	```
	vi /etc/crontab 파일 편집으로 작성 > * * * * * 유저명 커맨드
	또는 
	crontab 옵션 
	-e > 작성 및 편집
	-l > 예약 작업 확인
	-r > 예약 작업 전체 삭제
	```

	.---------------- minute (0 - 59)    
	|  .------------- hour (0 - 23)    
	|  |  .---------- day of month (1 - 31)    
	|  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...   
	|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat     
	|  |  |  |  |
	\*   *  *  *  * user-name  command to be executed    
	분 시 일 월  요일   사용자계정   명령어    
	3 9 3 * * nasir17 ps -aux > ~/pslis_”$(date ‘+%y%m’)”.txt    
	*/5 15 * * 2 유저명 할일   
	0 8,19 1-7 * 3 유저명 할일    

	메타 문자 사용 가능    
	\* 전체, - 범위지정, / 조건에 대한 주기    

### 소유권 변경

`chown 소유자:소유그룹 파일명또는디렉토리명`

### 디스크 장치 추가하기

- 장치인식 > 파티셔닝 > 파일시스템 생성 > 마운트

**파티셔닝**

- Partitioning, 추가된 장치의 용량을 사용할 만큼 나눔

- 파티셔팅 툴 : fdisk, gdisk, parted (parted gui)
    - fdisk, gdisk  대화형으로 가능, 명령어가 한 문자  (MBR , 요즘은 gpt)
    - parted : 대화형 / 쉘 둘다 가능.  (MBR, GPT)

	```
	ls -l /dev/sd*   장치인식 확인
	blkid  : 장치의 UUID  확인
	lsblk :  장치 목록 ( 마운트 포인트 등)
	```

**파일시스템 생성**

	```
	mkfs -t xfs -d name=/dev/sdc1
	mkfs.xfs /dev/sdc1
	-f 옵션으로 강제 생성 가능
	```

**마운트**

1. 수동 마운트

	```
	mount 장치경로 마운트포인트
	mount /dev/sdc1 /mounttest
	마운트해제는 unmount 장치경로 또는 마운트포인트
	```

2. 자동 마운트

	```
	blkid 명령어로 장치의 UUID 확인
	vi /etc/fstab 에서 필요한 정보를 기입하여 자동마운트 입력
	```

**fdisk 파티셔닝**

	```
	fdisk 옵션 디렉토리경로
	fdisk -l 현재 파티셔닝 상황 조회
	```

1. fdisk /dev/sdc > fdisk 파티셔닝 시작
2. m 또는 help > 도움말 조회
3. o > MBR로 설정 / g > GPT로 설정 (파티션 테이블 라벨 생성)
4. p > 파티션 테이블 라벨 조회
5. n > 신규 파티션 설정
6. t > 파티션 타입 변경
7. d > 파티션 삭제
8. w > 파티션 설정 정보 저장
9. q > fdisk 종료

**parted 파티셔닝**

	```
	parted 옵션 디렉토리경로
	parted -l 현재 파티셔닝 상황 조회
	```

1. parted /dev/sdc > parted 파티셔닝 시작
2. help > 도움말 보기
3. mklabel 라벨명 > 라벨 생성
4. mkpart > 파티션 생성
5. print > 작업완료 후 결과 확인
6. quit > parted 종료

### Swap


### Logical Volume

- Logical Volume : 유연한 스토리지 관리 기능을 제공함.
- 장점 : 
    - 파티션의 구조와 상관없이 원하는 크기의 논리볼륨을 생성 가능 
    - 논리볼륨이 부족시, 볼륨 확장 가능 (데이터 유지한 채로)
    - 데이터를 유지한 상태에서 논리볼륨을 구성하고 있는 디스크 제거 가능
    - RAID 적용한 볼륨 생성 가능
    - 스냅샷 (snapshot)  - 특정 시점의 데이터 보존 가능

- 디스크장치 여러개 장착 > 1개 파티셔닝 >  Physical volume 생성 > Volume Group  생성 > Logical Volume 생성

시스템을 끄고, disk 장착 > 부팅 > fdisk로 파티셔닝 (type : linux lvm) > pv 생성 > vg 생성 > lv 생성 > 파일 시스템 생성 > 마운트

### RAID

+ 씬 프로비저닝

### Systemd unit

#systemctl list-units   
journalctl    

### 부팅 프로세스

1. 부팅 과정

	- 부팅 절차를 왜 알아야하나?
		- 시스템 부팅중에 문제 발생시 해결하기 위해.
	- init 부팅 과정
	- systmed 부팅 과정

2. target  종류
	1. default.target
		- 부팅 후 가장 먼저 실행되는 target으로 연결되는 심볼릭 링크 파일
			- `#ls -l /etc/systemd/system/default.target`
		- 주로 multi-user.target 또는 graphical.target으로 연결됨
	2. graphical.target
		- 그래픽 환경 GUI의 다중 사용자 모드 , runlevel 5
		- 사용자화 가능
		- 시스템이 부팅될 때 graphical.target 단계에서, /etc/systemd/system/graphical.target.wants 내에 존재하는 unit을 실행함.
		- target 유닛 파일의 Requires/After 옵션에 의해 multi-user.target 지정되어 있음.
			- multi-user.target을 먼저 활성화 해야함
		- multi-user.target 환경과 유사
		- Server with GUI 또는 GNOME Desktop 패키지 설치해야함
	3. multi-user.target
		- 커맨드 환경CLI의 다중 사용자 모드 , runlevel 2/3/4
		- 사용자화 가능
		- init 프로세스의 런레벨 3과 맵핑됨.-> runlevel3.target도 가능
		- 시스템이 부팅될 때 multi-user.target 단계에서 /etc/systemd/systemd/multi-user.target.wants 내에 존재하는 unit을 실행함.
		- target 유닛 파일의 Requires/After 옵션에 의해 basic.target이 지정되어 있음 (basic.target이 먼저 실행 필요)
		- /(루트), /etc/fstab 마운트
		- 대부분의 서비스 실행
		- NIC 활성화, 여러 사용자 로그인 가능
		- 그래픽 도구 사용 못함.
	4. basic.target
		- firewalld, microcode, SELinux, 커널 메시지와 관련된 서비스를 시작하거나 모듈을 로드(Load) 함
		- 시스템이 부팅될 때 basic.target 단계에서 /etc/systemd/system/basic.target.wants 내에 존재하는 unit을 실행함.
		- target 유닛 파일의 Requires/After 옵션에 의해 sysinit.target이 지정되어 있음 (sysinit.target이 먼저 실행 필요)
	5. sysinit.target
		- 사용자 사용 불가
		- 시스템 마운트, 스왑, 커널의 추가 옵션을 실행하는 서비스를 시작.
		- 시스템이 부팅될 때 sysinit.target 단계에서 /etc/systemd/system/sysinit.target.wants 내에 존재하는 unit을 실행함 
		- target 유닛 파일의 Requires/After 옵션에 의해 local-fs.target이 지정되어 있음 (local-fs.target 먼저 실행 필요)
	6. local-fs.target
		- /etc/fstab에 등록된 마운트 정보로 파일시스템을 마운트 함.
		- target 유닛 파일의 After 옵션에 의해 local-fs-pre.target 지정.
			- local-fs-pre.target 먼저 활성화 필요
	7. poweroff.target
		- 시스템 shutdown -> poweroff  , runlevel 0
		- 실행 중인 서비스만 종료 (간단/빠르게 종료)    
		`#systemctl poweroff`    
		`#shutdown -h now`
	8. reboot.target
		- 시스템 shutdown -> reboot , runlevel 6    
		`#systemctl reboot`    
		`#shutdown -r now`    
	9. emergency.target
		- 긴급 쉘 emergency shell. 
		- 가능한한 최소한의 환경 제공. (root 인증 필수)
		- 부팅 중 오류 발생 시 rescue.target 부팅됨 -> 복구 쉘 -> 트리블슈팅
		- rescue.target 실행전 오류 발생시 emergency.target 지정 -> 복구 쉘 -> 트러블슈팅
		- /(루트) : 읽기 전용ro 마운트 (그외 unmount)
		- 네트워크 비활성화. 최소한의 서비스만 활성화
		- 시스템 파일 수정시 읽기쓰기rw로 리마운트 필요
	10. rescue.target (복구)
		- 복구 쉘 rescue shell  , runlevel 1
		- 사용자 가능 (단일 사용자, root 인증 필수)
		- 부팅 시 부팅을 완료할 수 없을 때 사용
		- sysinit.target, rescue.service 활성화되어야 rescue.target활성화됨.
		- /(루트) : 읽기쓰기로 마운트
		- 모든 파일 시스템을 마운트 시도, 중요한 서비스 시작함.
		- NIC 비활성화, 여러 사용자 로그인 안됨.

3. default target 설정 (시스템을 켰을 때, 어떤 타켓으로 부팅을 하겠는지 기본 값)
	- 런레벨 확인 #who -r
	- default.target 확인 #systemctl get-default
	- default.target 설정
		```
		#systemctl set-default multi-user.target
		#systemctl set-default graphical.target
		#shutdown -r now
		```

	- 지금 현재 target unit 전환    
   `#systemtcl isolate multi-user.target`     

4. 부팅 과정 중 target unit 지정
	- boot loader  중단 e(편집) -> linux16 라인 마지막에 systemd.unit=target-unit입력 
	- -> ctrl+x (실행) 부팅이어서


### 부트 로더 (GRUB)

- root 암호 재설정
	1. 시스템 재부팅
	2. 부트 로더 (boot loader) 중 화살표 (카운트 중단) -> 커널이미지 선택 (가장 최신 커널이미지) -> e 커널항목 수정
	3. linux16 라인 끝에 -> rd.break -> ctrl+x    (램디스크 초기화를 중단)
	4. "/" 읽기전용 마운트라서 읽기쓰기로 remount
		```
		#mount | grep -w '/sysroot'
		#mount -o remount,rw /sysroot
		#chroot /sysroot   (루트 디렉토리 변경)
		```
	5. 암호 변경 #passwd
	6. label 재지정 (chroot 명령어 입력시 레이블정보가 제거됨)
		```
		#touch /.autorelabel
		#exit
		#reboot (재부팅)
		```

systemd-analyze 명령어 : systemd 분석

### 패키지 관리

**소프트웨어 패키지  : rpm , yum**    

처음에 프로그램 설치 시 : 아카이브 파일이나 압축파일 다운로드 -> 원본 소스 파일 추출 -> 컴파일 -> 실행파일을 설치   :번거로움    	

쉽게 설치하기 위해 소프트웨어 패키지 (software package) : 특정 서비스를 운영하기 위해 필요로 하는 프로그램 또는 도구를 쉽게 설치하고 관리 할 수 있도록 하나의 패키지로 묶어서 제공하는 것.    

#### rpm

RPM : redhat package manager 패키지 관리 도구    

```
RPM 패키지 파일 형식 : 
	httpd-2.4.6-40.el7.centos.x86_64.rpm
	httpd - 2.4.6 - 40.el7.centos . x86_64 . rpm
	패키지이름-버전정보-릴리즈정보.아키텍처정보.파일확장자
	openssh-7.4p1-21.el7.x86_64
```

*종속성(의존성) : 어떤 패키지를 사용하기 위해 특정 패키지나 라이브러리 파일이 필요함 (사진 설치)    

패키지 확인 
```
#rpm -q [query-option] [query-argument]
-q with	-a all
		-f 파일이나 디렉토리
		-c 설정파일
		-d 문서파일
		-i  자세한 정보 확인
		-l 파일 목록 확인
		-s  파일 상태 확인
		-R 종속성  패키지 확인

#rpm -qa
#rpm -qf /var/log
#rpm -qc crond
#rpm -qd sshd
#rpm -ql sshd
#rpm -qs crond
#rpm -qi atd
#rpm -qR sshd
```

**yum**  
- Yellowdog Updater Modified
- RPM 기반의 패키지의 설치, 제거 그리고 업데이트를 관리하는 도구
- Repository레포지토리(저장소)에 패키지를 저장하고 관리하기 때문에 업데이트를 쉽게 함
	- Repository : 패키지들을 저장해놓은 하나의 서버
	- /etc/yum.repos.d/*.repo
		```
		 ex) /etc/yum.repos.d/test.repo
			[repoID]
			name=
			mirrorlist= 또는 baseurl=http://
			gpgcheck=1
			gpgkey=file:///etc/pki/rpm-gpg/keyfilename

		#yum repolist  (활성화되어있는 저장소만 확인)
		#yum repolist  all
		```
패키지 정보 확인 
- #yum info httpd  
- 상세 정보(패키지 이름, 버전, 릴리즈버전, 아키텍처, 파일 크기, 설치 유무)
- 특정 파일과 관련된 패키지 확인 #yum provides /etc/ssh/sshd_config

패키지와 관련된 키워드 검색 
- #yum search apache   이름과 summary만 매치하는 패키지를 출력
- #yum search all apache    이름,summary, description에서 키워드와 매치하는 패키지 검색

repository 패키지 목록 확인 
- #yum list
	- subargument : 	all 모든 패키지 목록 확인
		- available 현 repository에서 설치가능한 패키지 목록
		- extras 설치 가능한 설정 파일이 없는 패키지 확인
		- installed 설치된 패키지
		- obsoletes 설치된 패키지 중 repository에서 폐기된 패키지
		- recent 최근에 repository에 최근 추가된 패키지
		- updates 현 repository에서 업데이트가 가능한 패키지

패키지 설치 
- #yum install -y 패키지명
	- -y : 설치과정중에 발새하는 대화형 질문에 모두 yes처리
	- -d : 설치하지 않고 다운로드만
- 그외 시스템 내에 다운로드되어있는 rpm 패키지 설치도 가능
- URL 주소를 통한 yum 설치도 가능

패키지 업데이트 
- #yum update -y 패키지명
	- 하나의 소프트웨어 패키지에 대해서 다수의 버전을 설치할 수 없음.
	- 이전버전의 패키는 삭제되고 최신버전의 패키지가 설치됨.
	- update 시 특정 패키지를 지정하지 않으면 설치된 모든 패키지와 yum repository에 저장된 모든 패키지 버전을 비교하여 업데이트 진행함.
	- #yum install httpd

커널을 업데이트 할 때에는 업데이트 진행 중 오류가 난다면 부팅이 안됨.    
이전 버전의 커널을 삭제하지 않고 그대로 보존함. boot loader에 커널 목록이 추가됨.    

패키지 제거 
- #yum remove httpd

```
그룹 패키지 #yum groups [sub] [패키지명]
	sub : 	info	패키지 그룹 정보 확인
		install	패키지 그룹 설치
		list	패키지 그룹 목록 확인 (이름만 확인)
		remove	패키지 그룹 제거
 #yum groups info 'Basic Web Server'
```

패키지 설치 기록(history)
- yum으로 패키지 설치, 업데이트, 제거 등 로그파일에 기록됨. 
	- /var/log/yum.log  : 순자적 기록 , 패키지 그룹에 해당하는 모든 패키지 기록 됨.
	- #yum history   : 명령어 단위로 출력됨.
```
#yum history list
#yum history list 6
	yum 명령어 확인 가능
#yum history info 6	
패키지명이나 함께 설치된 패키지도 확인 가능
```

패키지를 yum으로 패키지를 설치하다가 network 오류등으로 설치 중단이 되었을 때, 기존 설치하던 자료는 삭제를 하고 다시 설치할 때에는     
	# yum clean all  (버퍼에 있는 정보 삭제)


### Network Manager

네트워크 정보 확인
```
ifconfig
ip addr
ping [option] destination
traceroute [option] destination
tracepath [option] destination
route -n 
```
```
*인터페이스 이름 : 장치유형  + 어댑터 유형 + 번호
 en(ethernet이더넷), w (wlan무선랜)
 o (on-broad) , s (hot-plug-slot) , p (PCI위치), b(BCMA Bus core) , ccw (CCCW bus group)

$ls /etc/sysconfig/network-scripts/
```

레거시 legacy 네트워크 구성 (파일 수정)		 

- 레거시 사용하는 경우, NetworkManager를 중지/비활성화/mask  (stop,disable,mask)
	- $ systemctl stop NetworkManager
	- $ ls -l /etc/sysconfig/network-scripts/ifcfg-*   파일 수정 > 네트워크 서비스 재시작

```
$ sudo vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
 속성	BOOTPROTO : 네트워크 관련 정보를 설정하기 위한 방식 , bootp, dhcp, none
static 구성인 경우 (BOOTPROTO: none) 구성사항
```

네트워크관리자 NetworkManager    
- nmcli : CLI 명령어
- nmtui : CLI환경에서 GUI처럼 키보드로 선택
- nm-connection-editor : GUI명령어

패키지 설치 : 
-  `yum -y install NetworkManager`
- `systemctl status NetworkManger`   

/etc/sysconfig/network-scripts 디렉토리의 설정파일을 저장함. (ifcfg-인터페이스이름)         

`$ ls -l /etc/sysconfig/network-scripts/ifcfg-* `   

nmcli명령어를 정리할것


연결정보 추가 할 때 명령어 형식    

```
nmcli con add con-name "connection 이름" ifname 물리장치 type ethernet autoconnet yes => dhcp 설정

nmcli con add con-name "connection 이름" ifname 물리 장치 type ethernet autoconnet yes ip4 "xxx.xxx.xxx.xxx/xx" gw4 "xxx.xxx.xxx.xxx" => 고정 아이피 설정

nmcli con reload

nmcli con up "connection 이름" 
```

### hostname

- IP주소를 보다 쉽게 구분하기 위해 시스템 이름 지정 > DNS 서비스
- FQDN (Full Qualified Domain Name): 정규화된 도메인 이름 (호스트네임+도메인네임)

호스트이름 분류
- static :  사용자가 지정한 정적인 호스트이름  , /etc/hostname에 저장
- transient : 커널이 유지 관리하는 동적 호스트 이름 (static 보다 우선순위가 낮음)
- pretty : 자유형식의 UTF8로 인코딩된 호스트이름, 길이와 문자 제한이 없으며, 특수문자 표현이 가능함.

```
hostnamectl
hostnamectl set-hostname "encore's linux"
hostnamectl status 
```

### 방화벽

방화벽 : 내부를 외부로부터 보호하기위해서 사용
- hardwre/software 가능.
- 내부가 외부로 가는 트래픽은 기본적으로 허용.
- 외부가 내부로 들오는 트래픽은 기본적으로 거부.
- 서버가 서비스를 하기위해서는 반드시 해당 서비스의 포트를 허용해주는 정책이 추가되어야함.

- 리눅스 방화벽은 커널의 netfilter 모듈에 의해서 패킷이 걸러짐
- 예전버전에는  iptables로 방화벽 설정
- centos 7는 systemd의 firewall-cmd (GUI:firewall-config)로 방화벽 설정.

firewall-cmd 명령어 정리

```
firewall-cmd

--state                                   : firewalld 실행 상태 확인
--get-default-zone                        : 현재 기본 영역 표시
--set-default-zone [zone]                 : 기본 영역 설정
--get-zones                               : 사용가능한 모든 영역 나열
--get-services                            : 사용가능한 모든 서비스 나열
--get-active-zones                        : 현재 사용중인 모든 영역과 인터페이스 및 소스정보 나열
--add-source=[ip주소] --zone=[zone]       : 출발지 주소 규칙 추가
=> '--zone' 옵션을 통해 zone 지정해주지 않으면 자동으로 기본영역에 추가
--remove-source=[ip주소]                  : IP 주소를 지정된 영역에서 제거
--add-interface=[ifname] --zone=[zone]    : 특정 영역에 interface 연결 추가
--change-interface=[ifname] --zone=[zone] : 영역에 연결된 interface 변경
--list-all --zone=[zone]                  : 영역에 구성된 모든 인터페이스, 소스, 서비스, 포트 나열
--add-service=[service] --zone=[zone]     : 해당 서비스 트래픽 허용
--add-port=[port|protocol] --zone=[zone]  : 해당 포트나 프로토콜 트래픽 허용
 + --permanent                            : 해당 옵션을 사용하지 않으면 현재 설정이 변경되며 영구설정은 지정이 안됨.
--reload                                  : 런타임 구성 삭제, 영구 구성 적용
--runtime-to-permanent                    : 실행중 설정을 영구 설정으로 변경
```