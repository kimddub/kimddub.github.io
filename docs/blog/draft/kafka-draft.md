---
layout: default
title: draft-5
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/draft-5
nav_order: 3
---

kafka
===========


### 설치 후 사용법

1. config/zookeeper.properties 수정
```
dataDir=C:\kafka_2.13-2.7.0\zookeeper-data
clientPort=2181
initLimit=5
syncLimit=2
server.1=[서버IP]:3888
```
 
2. 호출 서버의 dataDir 경로에 myid 파일 생성 후 서버 [고유ID] 작성(ex.1)

3. 호출 서버의 config/server.properties 수정
```
broker.id=[고유ID]
advertised.listeners=PLAINTEXT://127.0.0.1:9092
zookeeper.connect=localhost:2181
log.dirs=C:\kafka_2.13-2.7.0\kafka-logs
```

4. 새 프롬프트로 설치 경로에서 명령어 실행
```
# 주키퍼 시작
$ .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
#주키퍼 종료
$ .\bin\windows\zookeeper-server-stop.bat .\config\server.properties
```

5. 새 프롬프트로 설치 경로에서 명령어 실행
```
# kafka 시작
$ .\bin\windows\kafka-server-start.bat .\config\server.properties
# kafka 종료
$ .\bin\windows\kafka-server-stop.bat .\config\server.properties
```

6. 새 프롬프트로 설치 경로에서 명령어 실행
```
# 토픽생성
$ .\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic TestTopic

# 토픽확인
$ .\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181 
```


- - -

*참고: https://team-platform.tistory.com/13
*myid error 참고: https://www.thegeekstuff.com/2016/10/zookeeper-cluster-install/