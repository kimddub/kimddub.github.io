---
layout: default
title: draft-3
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/draft-3
nav_order: 3
---

TIL
===========

### server 이중화/ 클러스터링
서비스의 연속성을 유지하기 위한 구성.
이러한 구성을 이중화 HA(High Availabilty) 고가용성 이라 한다.

운영서버, DB서버 등을 이중화(HA)하여 구성하면, 운영 서버가 장애 시 대기 서버가 해당 서비스를 대신 운영한다.

구성 방법은 *공유디스크(스토리지 서버)*를 중심으로 접근하는 인스턴스 서버들을 *클러스터(집단화)* 구성하여 2개 이상의 시스템을 이중화한다.

DB 클러스터링 방안
- HA(High Availability)
    동일 장비를 Active 1대, Standby 1대로 구성하여 Active 장애 시 Standby로 서비스하는 방식. 각자 별도 storage를 가짐.
    - Active와 Standby 데이터 동기화 문제 및 성능 저하
    - Active가 죽고 Standby 전환하는 사이 트랜잭션은 유실됨
- OPS(Oracle Parallel Serve)
    storage 하나를 두고, 접근하는 인스턴스를 두개로 띄우는 방식. 하나의 instance 장애 시 다른 instance로 접근한다.
    - instance1에서 commit 한 데이터를 instance에 띄우려면 storage에 저장 후 instance2로 복제해야 함.
- RAC(Real Application Cluster)
    storage 하나를 두고, 접근 인스턴스를 여러대 두는 방식. OPS와 동일.
    다른 instance에서 commit한 데이터가 디스크를 거치지 않아도 바로 동기화 할 수 있는 Cache Fusion 지원.

DB 서버 이중화의 경우 로컬 DB의 변경내용을 원격 DB에 복제하여 관리하는 것으로 DB 무정지 서비스가 가능하다.
- 사용자는 하나의 DB에 대해서만 작업을 수행한다.
- 이중화 시스템에 연결되어있는 다른 DB에도 작업내용이 동일하게 적용된다.
- 여러개의 DB를 동시에 관리한다.


*참고 : <https://12bme.tistory.com/322>*

- - - 

### Kafka
스트리밍 데이터를 다루기 위한 미들웨어와 그 주변 생태계를 말한다. 많은 서비스에서 백엔드로 사용되고 있는 SW.
- 높은 확장성
- 가용성
- 데이터 영속성
- Pub/Sub 모델(Publisher, Subscriber 디자인 패턴)을 사용한 데이터 분포 지원
    Observer와 Subject가 서로를 인지하고 접근하는 Observer 패턴과 달리, Publisher와 Subscriber가 서로를 전혀 모른채 메시지 Broker(Queue)를 통해 접근한다.

- 설치방법


- - -

### Docker