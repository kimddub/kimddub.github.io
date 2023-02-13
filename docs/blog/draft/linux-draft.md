---
layout: default
title: draft-4
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/linux
nav_order: 3
---

===========
# Linux

### sudo
1. /etc/sudores 파일에 root 권한을 위임받도록 계정 설정
2. 설정된 계정만 sudo 명령어로 root 권한이 사용하는 80 포트 등 사용 가능

### 옵션
옵션의 큰 분류
1. '-'를 맨 앞에 붙여 그룹지어 사용하는  유닉스 옵션
2. '-'없이 그룹지어 사용하는 BSD 옵션
3. '--'를 맨 앞에 붙여 사용하는 기다란 GNU 옵션

### 명령어
```
# 서버 재부팅
sudo reboot

# 서버 서비스 관련 파일 
cd /etc/init.d
cd /etc/systemd/system

# 실행중인 java 관련 프로세스 조회
ps -ef | grep java
# 실행중인 프로세스 명으로 조회
ps -fu | grep [프로세스명]
# 실행중인 프로세스 포트로 조회
netstat -anp | grep [포트]

# 네트워크 인터페이스
sudo netstat -nplt

# 방화벽 확인
telnet [IP] [PORT]
netstat -an | find "[IP]:[PORT]"
```

### mysql
1. 덤프
```
mysqldump -u root -p --default-character-set=utf8  --databases DB_OLP > DB_OLP.sql

# 원격서버 덤프
mysqldump -u kpst_deid -pkpst_deid_123 --default-character-set=utf8 -h 10.100.30.13 --databases db_sejong > db_sejong.sql

mysqldump --no-tablespaces -u DB_CCP_ADMIN -pDb_ccp_1324! --default-character-set=utf8 -h 10.100.30.103 --databases DB_CCP > DB_CCP.sql

mysqldump -u olp -pDb_olp_1324! --default-character-set=utf8 -h 10.100.30.123 --databases DB_OLP > DB_OLP.sql

mysqldump --no-tablespaces -u oas -pDb_oas_1324! --default-character-set=utf8 -h 10.100.30.63 --databases DB_OAS > DB_OAS.sql

mysqldump --no-tablespaces -u eam -pDb_eam_1324! --default-character-set=utf8 -h 10.100.30.123 --databases EAM_ADM > EAM_ADM.sql
***안됨***

```

2. 백업 스케줄러
https://stricky.tistory.com/304

```
# mysql db backup shell 
0 4 * * 0 /home/mysql/mysql_dump/script/db_backup.sh &
```

3. 사용자 추가
create user 'eam'@'%' identified with caching_sha2_password by 'Db_eam_1324!';
grant all privileges on EAM_ADM.* to 'eam'@'%';
