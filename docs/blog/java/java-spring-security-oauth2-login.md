---
layout: default
title: "[Java] Spring Security - OAuth2 개념"
grand_parent: Blog
parent: java
permalink: /docs/blog/java/java-spring-security-oauth2-concept
nav_order: 1
date: 2023-02-13 10:04
---

## 스프링 시큐리티 OAuth2

### 개념
- SecurityBuilder
  - 웹 보안 구성하는 Bean 객체와 설정 클래스 생성
  - 종류
    1. WebSecurity
       - 우선 동작
       - FilterChainProxy 반환(SecurityFilterChain을 포함함)
    2. HttpSecurity
       - SecurityFilterChain 반환
       - SecurityFilterChain: 인증 인가하는 Filter 클래스
- SpringConfigurer
  - HTTP 요청 관련 보안처리 필터 생성 및 초기화 설정에 관여
  - 인증 및 인가 초기화(SecurityBuilder에 포함됨)
  - 작동 순서
    1. 빌더 클래스 생성   
       `AutoConfiguration -> SecurityBuilder.build()`
    2. 설정클래스 생성
    3. 초기화 작업 진행  
       `SpringConfigurer.init() & SpringConfigurer.configure()`
- SecurityContextHolder 
- SecurityContext
  - 접근 주체와 인증 정*(Authentication)를 담고 있다.
- Authentication
  - Principal과 GrantAuthority 제공. 인증 성공 시 Athentication 생성 및 저장됨.
- Principal
  - 유저 정보로 대부분의 경우 Principal로 UserDetails를 반환.
- GrantAuthority
  - ROLE_ADMIN, ROLE_USER 등 Principal이 가지고 있는 권한이며 prefix(ROLE_) 설정 표준이 있음.

### 커스텀 설정 클래스
- SecurityConfig
  - 커스텀하지 않을 경우 기본 설정 클래스 구동 위치  
    `SpringBootWebSecurityConfiguration.defaultSecurityFilterChain()`
  - 설정 방식 
    1. SecurityFilterChain
    2. *WebSecurityConfigurerAdapter -> deprecated*

### Client Type 및 권한부여 유형
- 기밀형 Client VS 공개형 Client
  - SPA와 같이 백엔드와 분리된 프론트엔드로 구성된 클라이언트는 노출되며 이 경우 access-token을 클라이언트에 직접 제공하면 보안에 취약하다.

### 권한부여 유형
- Authorization Code Grant
    - 클라이언트 프론트가 코드를 발급받아 클라이언트 백엔드로 전달
    - 클라이언트 백엔드가 해당 코드로 access-token을 발급받음
- Authorization Code Grant with PKCE
    - 공개형 Client의 경우 코드 탈취로 인한 악용도 가능하므로 코드값도 PKCE로 암호화하는 고도화된 방식
- 이외 공개형 클라이언트에서는 사용이 지양되는 권한부여 유형들이 추가로 더 존재한다.

### 프로토콜
- OIDC(OpenID Connect) 1.0
  - 인증(Authentification) 수단으로써의 OAuth 2.0 최상위 레이어
  - ID 토큰(JWT) 반환
  - 토큰 관련 엔드포인트 및 공개 키 정보 제공(/.well-known/openid-configuration)
- OAuth 2.0
  - 인가(Authorize)

### ID Token VS Access Token
- ID Token
  - 사용방식
    - 사용자 신원확인에 사용
    - API 요청에 사용 불가
  - 발급방식
    - ID Provider에서 발급
    - ID Token(JWT) 디코딩하면 사용자 정보를 얻을 수 있음
  - 의문점
    - access_token 디코딩 시에도 신원정보 유사하게 제공하지만 인증 목적이 아닌 토큰?
    - id token은 리소스 요청 시 신원정보를 응답값에 담아보내는 위험성을 최소화하기 위함?
- Access Token
  - 사용방식
    - 리소스(API) 접근에 사용
    - 인증에 사용 불가
  - 발급방식
    - 인증 서버에서 발급

### Scope
- Resource Owner가 허가한 자원 정보의 범위를 의미
- OIDC Scope
  - openid, profile, email, address, phone, ...
