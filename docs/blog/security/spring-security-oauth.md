---
layout: default
title: "[Security] Spring Security OAuth"
grand_parent: Blog
parent: java
permalink: /docs/blog/java/spring-security-oauth
nav_order: 1
date: 2023-05-16 10:04
---

## Spring Security OAuth

### 개념
- OAuth(Open Authorization)
  - 위임 `권한` 부여를 위한 표준 프로토콜. 사용자 PW없이도 사용자의 데이터 접근을 허가함.
  - 인증 플로우 2가지
    - Server Side Application 플로우
    - 브라우저 베이스의 앱을 위한 묵시적 플로우
  - OAuth2.0
    - Resource Owner: 클라이언트 어플리케이션이 접근할 데이터의 소유자
    - Client: 사용자 데이터에 접근하려는 어플리케이션
    - Authorization Server: 사용자로부터 권한을 부여받고 클라이언트에게 권한을 부여하는 서버
    - Resource Server: 클라이언트 어플리케이션이 접근할 데이터 저장 시스템. 권한부여 서버와 같은 경우도 있음
    - Access Token: 데이터 권한을 부여받은 클라이언트가 Resource Server 데이터에 접근하기 위한 인증 키 
- OpenID Connect - Authentication
  - `인증`을 위해 사용되는 프로토콜
  - OAuth를 권한에 대한 유즈 케이스들에 적합하게 만들기 위한 OAuth2.0 프로토콜 상위 레이어에 있음
- 위임 권한부여(Delegated Authorization)
  - 서드파티 어플리케이션이 사용자 데이터에 접근하도록 허락해주는 것
  - 접근법 2가지
    - 서드파티 어플리케이션에 ID/PW를 제공하여 사용자 대신 로그인 및 데이터에 접근토록 하는 것(사용하지 않는 방식)
    - OAuth를 통해 서드파티 어플리케이션에게 사용자 데이터 접근 권한을 주는 것
> 참고: [jakeseo_me님 블로그](https://velog.io/@jakeseo_me/Oauth-2.0%EA%B3%BC-OpenID-Connect-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%A0%95%EB%A6%AC)
- JWT
  - JWT는 인증 토큰의 종류, OAuth는 토큰을 발급하고 인증하는 오픈 스탠다드 프로토콜
- Spring Security Fundamentals
