---
title:  'W04 리눅스 서비스'
categories: 'review'
---

4주차에 진행된 Linux Service 과정의 종합 정리.

실제로 서버에 올라가는 다양한 서비스 구성 방법을 배우고 실습하였다.

**주요 키워드**

- ssh
- DNS
- Apache (WEB)
- MariaDB
- HTTPS
- NFS
- HAproxy

---


# SSH(Secure SHell) sevice

openssh    
Telnet의 암호화 된 버전

## 패키지 관리

- 설치 전 확인
    - rpm -qa openssh

- 설치 방법
    - sudo yum install - y ssh > 진행되지않는다, 패키지이름은 openssh
    - sudo yum install - y openssh > 로 진행해야 설치가능

- 설치 후 확인
    - rpm -ql openssh

- service unit 및 방화벽
    - systemctl status sshd.service / firewall-cmd —add-service=ssh

- 접속 방법
    - ssh 계정명@IP주소 > ssh encore@192.168.56.10
    - Key 인증을 통한 접속방법
        - 클라이언트를 통해서 공개키와 개인키를 생성하고, 공개키를 서버로 옮김.

1. 키 생성

    ssh-keygen -t rsa 

1. 키 복사

    ssh-copy-id 서버ID@서버IP주소 > ssh-copy-id encore@10.0.2.10

    ~/.ssh/id_rsa.pub가 서버의 ~/.ssh/authorized_keys 에 데이터가 전송됨

1. sshd_config 설정

    vim /etc/ssh/sshd_config > 65 passwordAuthenticaton yes-> no

4.  sshd.service daemon 재시작

- 명령어 scp 활용

```
scp source destination

scp 현재위치  목적지 

scp 목적지   현재위치
```

1) ssh user02@localhost

ls -lR > list.user02

scp list.user02  user03@localhost:~user03

2) ssh user03@localhost 또는 su – user03

mkdir ~/test

ls -lR etc  >  ~test/ etc.list

scp -r  \~/user03/test   user02@localhost:~user02

- 오류사항 정리

“Warning: Remote host identification has changed”

기존 key가 known hosts에 저장되어있는데 새로운 pc(mac addr)로 접속시도해서 발생

기존키를 삭제해서 해결

### DNS

### Apache (WEB)

### MariaDB

### HTTPS

### NFS

### HAproxy
