---
layout: default
title: "MSA 게이트웨이"
grand_parent: Blog
parent: msa
permalink: /docs/blog/msa/msa-3
nav_order: 1
date: 2023-09-05 10:00
---

# MSA Gateway
마이크로서비스와 같은 분산형 아키텍처에서 보안/로깅/사용자 추적 등 공통적인 횡단 관심사를 처리하기 위한 서비스
- 클라이언트는 서비스를 직접 호출하지 않는다.
- 게이트웨이 경유 후 최종 목적지로 라우팅한다.

## 게이트웨이 특징
- 장점
  - 정적 라우팅
    - 단일 서비스 엔드포인트(URL) + API 경로 호출
  - 동적 라우팅
    - 클라이언트 유형에 따른 라우팅 처리
  - 인증 및 인가
    - 서비스 호출자에 대한 인증, 인가를 공통적으로 처리
  - 지표 수집 및 로깅
    - 서비스 호출 정보나 사용자 요청 정보에 대한 지표를 균일하게 로깅하도록 보장
- 단점
  - 단일 장애 지점
    - 로드밸런서
      - LB는 게이트웨이 인스턴스 앞에 둘 때 게이트웨이 확장 등이 가능하다.
      - LB를 각 서비스 인스턴스 앞에 두는 것은 병목점이 될 수 있다.
      - 서비스 디스커버리를 사용하지 않는 경우 각 서비스 그룹 앞에 LB를 둘 수도 있다.
    - 무상태
      - 게이트웨이 메모리에 데이터를 저장하는 행위는 확장성을 제한한다.
      - 데이터는 모든 게이트웨이 인스턴스에 복제되어야 한다.
    - 성능
      - 여러 데이터베이스를 호출하는 복잡한 코드는 추적이 어려운 성능 이슈를 발생시킨다.

## 스프링 게이트웨이
논블로킹 게이트웨이로 백그라운드에서 비동기식으로 처리하고, 완료 시 응답을 반환한다. 또한, 주요 스레드를 차단하지 않는다.
- 구현 절차
  - 단일 URL
  - 필터(Filter)
  - 서술자 (Predicate)
- 기본 구성 방법
  - 게이트웨이 구성정보 저장
    - 게이트웨이 bootstrap.yml
      - 서비스 이름 지정
      - 컨피그 서버 위치 설정
  - 유레카 구성정보 설정 
    - 컨피그 서버 gateway-server.xml
      - 유레카 구성 정보 설정
    - 게이트웨이 BootClass
      - @EnableEurekaClient
- 라우팅 구성 방법
  - 서비스 디스커버리 자동 경로 매핑 설정
    - 컨피그 서버 gateway-server.yml
      - discovery.locator 설정 - 호출 ID만으로 인스턴스 자동 호출, 게이트웨이 수정없이 인스턴스 확장
  - 서비스 디스커버리 수동 경로 매핑 설정
    - 컨피그 서버 gateway-server.yml
      - routes - 서비스 ID와 URI 설정
      - routes.predicates - 수동 호출 엔드포인트 설정
      - routes.filters - 응답 전/후에 필터링 적용
- 동적 라우팅 구성 재로딩
  -  컨피그 서버 검색 대상 git 연동 시 서버 재시작없이 refresh 호출하여 구성정보 재로딩 가능
- 사용자 정의 로직
  - 보안, 로깅, 추적 등의 사용자 횡단 관심사를 정책으로 적용 가능
  - 게이트웨이 아키텍처
    - 게이트웨이 클라이언트 → 핸들러 매핑 → 웹 핸들러 → 필터 → 프록시 서비스
    - 핸들러 매핑: 요청 경로가 라우팅 구성과 일치하는지 확인
    - 웹 핸들러: 특정 경로에 구성된 필터 추적 및 요청 전달
  - Predicate Factories
    - 요청 실행 또는 처리 전에 조건이 서술집합을 충족하는지 확인
    - 시간 관련 서술자: Before, After, Between
    - 요청 관련 서술자: Header. Host. Method, Path, Query, Cookie, 
    - 물리정보 관련 서술자: RemoteAddr
  - Filter Factories
    - 정책코드 시행 시점을 일괄된 방식으로 처리
    - HTTP 요청/응답 수정
    - 매개변수 관련 필터: AddRequestHeader, AddResponseHeader, AddRequestParamter, RequestRateLimiter
    - 요청 관련 필터: PrefixPath, RedirectTo, RemoveNonProxy, RemoveResponseHeader, RewritePath, SecureHeaders, SetPath, SetSStatus, SetResponseHeader
    - 사용자 정의 필터
      - pre-filters: 요청 라우팅 전 호출하여 사용자 인증 등의 게이트키퍼 역할
        - (예)추적 필터: 유입되는 요청을 검사하고 HTTP 헤더에 상관관계 ID가 없는 경우 생성하여 함께 실행하여 모든 마이크로서비스에 전달되는 고유 ID로써 이벤트 체인 추적 가능
      - post-filters: 요청 라우팅 후 대상 서비스의 응답을 기록하거나 오류, 민감정보 등을 검사하기 위한 역할
        - (예)응답 필터: 클라이언트에게 응답될 HTTP 헤더에 상관관계 ID를 삽입하여 클라이언트 요청과 연결된 상관관계 ID에 엑세스 가능
- 사용자 정의 필터 구성 방법
  - GlobalFilter 인터페이스 구현 및 filter() 메소드 재정의
  - ServerWebExchange 객체에서 헤더 추출 비즈니스 로직 수행
  - ServerWebExchange mutate() 메서드로 헤더 수정 가능
- 상관관계 ID
  - 사용자 정의 필터를 통해 상관관계 ID를 조회/생성/삽입 하는 이유
    - 호출된 마이크로서비스가 쉽게 엑세스
    - 마이크로서비스로 수행될 모든 하위 서비스 호출 시 상관관계 ID 전파
    - 각 상관관계 ID로 호출 관련 모든 서비스 통과 트랜잭션을 추적 가능
    - ???
  - 게이트웨이를 통해 삽입된 상관관계ID를 각 서비스에서 UserContext 객체로 관리
## 키워드
- 상관관계 ID: ThreadLocal 변수로 생성된다.. 권한 토큰과는 별개.. 학습 필요..
- ThreadLocal: https://dev.gmarket.com/62 학습 필요..














