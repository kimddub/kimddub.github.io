---
layout: default
title: "MSA 준비"
grand_parent: Blog
parent: msa
permalink: /docs/blog/msa/msa-2
nav_order: 1
date: 2023-07-07 10:00
---

# MSA 준비

## 스프링 클라우드 요약
1. 스프링 클라우드 컨피그  
   환경 별 애플리케이션 구성 데이터를 자체 `프로퍼티 관리 저장소` 또는 `오픈 소스 프로젝트 통합` 등으로 관리한다.
   - Git: 백엔드 저장소
   - [Consul](https://www.consul.io/): 오픈 소스 서비스 디스커버리로 애플리케이션 구성 데이터를 키-값 저장소 DB를 통해 관리하며 서비스 인스턴스의 위치를 질의하여 찾는다.
   - [Eureka](https://github.com/Netflix/eureka): 오픈 소스 서비스 디스커버리로 콘술과 유사하다.
2. 서비스 디스커버리  
   클라이언트에서 서버가 배포된 물리적 위치 추상화 및 서비스 시작과 종료에 따라 인스턴스를 처리한다.
   - [Consul](https://www.consul.io/)
   - [Eureka](https://github.com/Netflix/eureka)
   - [Zookeeper](https://spring.io/projects/spring-cloud-zookeeper)  
   콘술과 주키퍼가 강력하고 유연하지만, 교재에서는 관리가 쉬운 유레카 활용
3. 회로 차단기(Circuit Breakers)     
   Spring Cloud Circuit Breaker는 Resilience4j, Sentinel, Hystrix 세 가지 옵션 중에서 선택 할 수 있으며,  
   Resilience4j 라이브러리를 사용하면 회로 차단기, 재시도, 벌크헤드 등 서비스 클라이언트 회복성 패턴 구현이 가능하다.
4. API Gateway  
   서비스 라우팅으로 호출에 대한 중앙 집중화하여 보안 인가, 인증, 필터링 등의 표준 서비스 정책을 시행한다. 다음의 프로젝트들을 사용하면 통합에 용이하다.
   - Spring Web Flux: 웹 요청을 Reactive하게 처리 가능하도록 함.(MVC 패턴과 거리가 멀다.)
     - 비동기 개발에 사용되므로 Netty 기반의 서버 필요
       - Tomcat: 1 request -> 1 thread 
       - Netty: n request -> 1 thread
   - Spring Framework 5 project Reactor
   - Spring Boot 2
5. Spring Cloud Stream
   데이터 중심 애플리케이션을 Kafka나 RabbitMQ와 빠르게 통합하도록 지원하고 단독 빌드/테스트 할 수 있다.
6. Spring Cloud Sleuth
   ELK(로깅 집계 도구) 및 Zipkin(추적 도구)와 결합될 때, 트랜잭션 추적 역할을 한다.
7. Spring Cloud Security
   서비스에 엑세스하는 사용자와 사용자의 작업을 제어하는 인증 및 인가 프레임워크이다.

## 프로젝트 세팅
### 0. 뼈대 만들기
[Spring initializr](https://start.spring.io/)
- Bootstrap Class
  - XXXApplication.java + @SpringBootApplication
  - 스프링 부트는 이 클래스가 bean을 정의하는 출처라고 컨테이너에 알림
  - 기본적으로 main 메소드
  - localeResolver, messageSource 등의 세팅하는 경우 bean 생성 위치
- Spring Bean
  - Spring IoC 컨테이너가 관리하는 자바 객체로서 컨테이너에 의해 생명주기가 관리되는 객체(ApplicationContext)
  - MVC 어노테이션이나 @Configuration, @Bean 어노테이션 또는 xml 설정을 통해 bean 등록 가능
  - Scope  
    하나의 bean 정의에 대해 객체가 존재하는 범위
    - singleton: 컨테이너에서 단 하나의 객체만 존재
    - prototype: 다수 객체 존재
    - request: 하나의 HTTP request 생명주기 안에 단 하나의 객체만 존재
    - session: 하나의 HTTP session 생명주기 안에 단 하나의 객체만 존재
    - global session: 하나의 global HTTP session 생명주기 안에 단 하나의 객체만 존재
  - [출처](https://developer-ellen.tistory.com/198)

### 1. REST API 서비스
- HATEOAS(헤이티오스, 헤이트 이 오 아스) 원칙
  - 정의
    - REST API를 구현하기 위한 원칙 
    - Hypermedia (링크)를 통해서 애플리케이션의 상태 전이가 가능해야 한다.
      또한 Hypermedia (링크)에 자기 자신에 대해한 정보가 담겨야 한다.
    - API를 조회한 후 클아이언트가 취할 수 있는 다음 행동(상태 전이)을 위한 API를 응답 본문에 추가한다.
    - 이렇게 되면 API 버전 명세, 링크 정보 변경 용이성, 링크 상태 전이성이 개선된다.
  - 실습
    - 의존성을 추가하여 MVC 프로젝트에 적용 할 수 있다.
    - 객체 정의 시 RepresentationModel을 extend하여 컨트롤러 단에서 해당 객체에 링크를 추가 할 수 있다.
    - 객체 초기화 후 본 메소드는 selfRel으로, 관련 메소드는 rel으로 각각 linkTo한다. 
    - link 할 때, controller의 메소드를 참조하여 현 메소드와 동일한 파라미터가 매핑된 api url을 지정한다.
  - 참고
    - 이러한 포멧을 지원하는 HAL JSON 타입이 있다.(application/hal+json,application/hal+xml)
- 왜 JSON을 사용하는가?
  - 표준
  - 가볍고 적은 텍스트로 사람이 이해하기 쉬운 데이터 표현이 가능
  - javascript가 프로그래밍 언어로 부상하면서 기존 직렬화 프로토콜로 사용되던 JSON도 상용화
  - (참고)Apache Thrift 프레임워크를 사용하면 바이너리 프로토콜로 통신하여 더 효율적인 다언어 서비스 구축 가능
- @RestController
  - 기존 @Controller와 달리 메소드에서 ResponseBody 클래스를 반환할 필요가 없다.
  - 엔드 포인트 최소한으로 노출하기
- 캡슐화
  - 객체지향을 위한 원칙으로 클래스의 변수를 private 선언 후, @getter와 @setter 활용
  - Lombok: 위 어노테이션을 대체하는 소형 라이브러리
- 엔드포인트
  - 리소스 명확히
  - 동사 미사용
  - 버전 체계 -> 왜 필요할까?
- 국제화(internalization)
  - 콘텐츠를 제공하는 애플리케이션의 다언어를 지원
  - Bootstrap Class에 Bean 생성 및 국적별 properties 파일 세팅
  - Controller 메소드에서 @RequestHeader(value="Accept-Language", ...)으로 Locale 파라미터로 받아 Service 단에 전달
  - MessageSource 객체에서 국적별 로컬라이징된 메시지 얻기
- 런타임
  - 데브옵스 관점에서 마이크로서비스는 조립 -> 부트스트래핑 -> 디스커버리 -> 모니터링 단계를 거친다.
  - 조립
    - 메이븐 스크립트의 빌드/배포 엔진으로 빌드 시작
    - 소스 코드 체크인에 따라 엔진이 빌드 및 패키징
    - 빌드 결과물은 런타임 컨테이너를 내장한 하나의 JAR 실행 파일
  - 부트스트래핑
    - 구성 정보 저장소에서 변경 사항 버전관리와 누가 언제 변경했는지 감독
    - 서비스 구성 정보 변경에 따라 기존 실행 서비스 종료와 새 구성 정보가 로드되도록 서비스에 전달
    - 마이크로서비스가 시작될 때, 환경 정보 및 애플리케이션 구성 정보 확인
  - 디스커버리
    - 서비스 인스턴스 시작될 때 자신을 디스커버리 에이전트에 등록
    - 서비스 클라이언트는 인스턴스의 물리적 위치(IP)를 모른 채 에이전트를 통해 찾는다.
  - 모니터링
    - 디스커버리 에이전트가 인스턴스 상태를 모니터링
    - 인스턴스가 고장나면 가용 인스턴스 풀에서 제거
    - 인스턴스는 디스커버리 에이전트가 호출할 수 있는 상태 확인용 URL을 노출
    - 이 URL에서 에러가 반환되거나 응답이 지연되면 에이전트가 인지하여 해당 인스턴스로 트래픽을 라우팅하지 않음
    - REST 환경에서 상태 확인 인터페이스는 HTTP 엔드포인트와 JSON 페이로드를 노출하는 것 
      - spring actuator가 서비스의 UP/DOWN 지표 및 기타 정보를 제공 

### 2. 서비스 디스커버리



## ETC. MSA 전환전략
> Q) 유레카 적용이 필수일까?
> Q) Spring Cloud가 아닌 애플리케이션도 @EnableEurekaClient(유레카 클라이언트 등록) 가능할까?   






