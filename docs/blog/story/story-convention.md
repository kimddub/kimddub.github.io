---
layout: default
title: "[개발] 코드 컨벤션"
grand_parent: Blog
parent: story
permalink: /docs/blog/story/story-convention
nav_order: 1
date: 2023-06-09 10:00
---

# 코드 컨벤션
프로젝트 개편 규모가 크다보니 신규 프로젝트 설계부터 들어가고 있다.  
2명이라는 인원으로 페어 프로그래밍을 진행하고 있는데, 설계 단계가 굉장히 중요하다고 느낀다.
따라서, 프로젝트(언어) 별로 다양한 측면의 컨벤션을 정의하기로 했다.

## REST API
<details>
<summary>상세내용</summary>

- URL Rules
  - 마지막에 / 포함하지 않는다.
  - _(underbar) 대신 -(dash)를 사용한다.
  - 소문자를 사용한다.
  - 행위(method)는 URL에 포함하지 않는다.
  - 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.
- Set HTTP Headers
  - Content-Location
  - Content-Type
  - Retry-After
    - Case 1. 인증
    - Case 2. 자원 요청
  - Link
- Use HTTP methods
  - POST, GET, PUT, DELETE 4가지 methods는 반드시 제공한다.
  - OPTIONS, HEAD, PATCH를 사용하여 완성도 높은 API를 만든다.
    - OPTIONS
    - HEAD
    - PATCH
- Use HTTP status
  - 의미에 맞는 HTTP status를 리턴한다
  - HTTP status만으로 상태 에러를 나타낸다
- Use the correct HTTP status code.
  - 성공 응답은 2XX로 응답한다.
  - 실패 응답은 4XX로 응답한다.
  - 5XX 에러는 절대 사용자에게 나타내지 마라
- Paging, Ordering, Filtering, Field-Selecting
  - Paging
    - Paging Key
    - 응답
      - HTTP Header의 Link 속성을 이용한다.
      - <s>HATEOAS로 응답한다.</s>
      - Link, <s>HATEOAS</s> 모두 사용한다.
  - Ordering
  - Filtering
  - Field-Selecting
- Versioning
  - 종류
    - 예외적으로 서비스의 기본 도메인이 3차인 경우 path level에 모두 명시한다.
  - URI Versioning
</details>

## JAVA
<details>
<summary>상세내용</summary>

- Structure
  - 패키지 구조
    - 계층형 + 도메인(member, job, interviewer, admin, common)
    - 컨피그 클래스 분리 - 부트스트랩 클래스에 ＠Enable 어노테이션 미권장 이유
  - 클래스 세분화
    - service - 도메인 별 서비스를 통합하지 않고 용도마다 분리
    - dto - 엔티티와 분리, 요청/응답 별 dto 분리
  - api 통신
    - FeignClient
    - Apache HttpClient
    - WebClient(RestTemplate)
  - 컨피그 파일
    - yml
  - Programing
    - 공통 엔티티
      - enum
      - ResponseDto
        - message
  - 예외처리
    - Custom Exception 지양
    - Java 기본 Exception + ErrorCode 메시지 잘 활용
  - 의존성 주입
    - 생성자 주입
    - <s>필드 주입(Autowired)</s> 지양
  - TDD
    - 미사용
    - 개발량 대비 학습 및 공수가 필요하여 ..
  - 유효성 체크
    - Valid 어노테이션 활용
  - Swagger
    - 최소한의 필수 기능만 사용
  - Locale 설정
  - Lombok
    - 기본적으로 ＠getter + ＠NoArgsConstructor 까지 사용
    - ＠Data 지양
  - 이외 통상적으로 사용하는 문법 지향
    - [캠퍼스 핵데이 Java 코딩 컨벤션](https://naver.github.io/hackday-conventions-java/)
</details>

## JS