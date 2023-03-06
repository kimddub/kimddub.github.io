---
layout: default
title: java
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/java
nav_order: 3
---

Postgresql
===========
# DB접속
- postgres
    sudo su -
    su - postgres
    psql -U postgres
# DB 재기동
```
service postgresql status
service postgresql restart
```
# dump
- postgres
    - backup
    pg_dump -d vaivsquare -h localhost -U vaivsquare -F t > /data/backup/vaivsquare_all_20220405.tar 
    - resote
    pg_restore -v -h localhost -U postgres -C -d cms -F t /data/cms_all_20211207.tar  
    pg_restore -v -h localhost -U postgres -d vaivsquare -F t /home/vaiv/data/backup/vaivsquare_all_20220405.tar 
    pg_restore -v -h localhost -U postgres -a -d cms -F t /data/cms_all_20211207.tar 
        - 첫번쨰 예 : Backup한 DB를 Restore하는 위치에 새로 생성하면서 복구. (이를 위해 -C option을 사용했으며, 이 경우에는 root 계정을 이용해서 restore해야한다. Target DB 명 없이 Backup한 Database 명을 그대로 사용한다)
        - 두번째 예 : Restore할 Database Instance, Schema가 이미 존재하는 상태에서 Database Object들을 복구.
        - 세번째 예 : 이미 Database내에 Object가 존재하는 경우, Table 내에 Data만을 복구.
        - Backup및 Restore시에 Machine이 마치 정지한 것처럼 보여질 수 있는데, 이를 막기 위해서는 -v Option을 활용해서 진행 상황을 확인할 수 있게 하는 것을 권장한다. 아래는 Restore시에 -v 옵션을 통해 중간 진행 상황을 확인하는 예이다.
# 오류 해결
- psql: FATAL: Ident authentication failed for user
    - /var/lib/pgsql/11/data/pg_hba.conf
        1. peer, ident -> md5 수정
        2. 설정 파일 reload
        select pg_reload_conf();

# postgresql 설치 메뉴얼(65)
1. 설치 확인및 키발급
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
sudo apt install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
2. 최신버전설치
sudo apt-get update
sudo apt show postgresql
sudo apt -y install postgresql postgresql-contrib
3. 설치확인
service postgresql status
4. 접속확인
sudo su - postgres
psql
postgres=# \conninfo
postgres=# \q
5. 비밀번호 변경
postgres=# alter user postgres with password 'Asdf!234'
6. 데이터베이스 생성
postgres=# create database "CMS" OWNER postgres;