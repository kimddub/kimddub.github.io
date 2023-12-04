---
layout: default
title: "MSA 보안"
grand_parent: Blog
parent: msa
permalink: /docs/blog/msa/msa-4
nav_order: 1
date: 2023-09-05 10:00
---

# MSA 
- 보호 계층
  - 애플리케이션 계층: 사용자 인증/인가
  - 인프라스트럭처
  - 네트워크 계층
- 취약성 발견 도구
  - OWASP: 의존성 검사 프로젝트로 공개된 취약점을 찾아내는 도구

## 사용자 인증/인가
### 구현수단
  - 스프링 클라우드 시큐리티 모듈
  - 키클록

## OAuth2
ID 제공자의 인증 서비스로 사용자를 인증하며, 모든 요청에 전달할 수 있는 토큰을 제공받아 유효성을 확인한다.
### Grant Type
- OAuth2 프레임워크는 각 클라이언트 환경에 적합한 인증/인가 방식을 제공한다.
- 클라이언트 인증 후 발급된 토큰은 만료 시점까지 모든 엔드포인트 요청 헤더에 첨부한다.
- 사용자 인증 과정에 개입하는 방식 2가지와 개입하지 않는 방식 2가지가 있다.
  - 사용자 인증 과정 개입
    - authorization code
    - implict
  - 사용자 인증 과정 미개입
    - password
    - client credentails
- 종류
  - authorization code
    - 클라이언트가 백엔드를 제공하는 웹 어플리케이션에 적합한 방법이며 access_token이 외부로 유출되지 않는다.
    - 프로세스
      1. 사용자가 권한 위임에 동의(로그인) 후 인증 서버에서 리다이렉트 페이지로 code 획득 
      2. 브라우저에서는 redirect_uri가 가르키는 클라이언트의 백엔드 웹 서버로 요청
      3. 백엔드에서 code를 통해 인증 서버에서 access_token 발급
      4. access_token으로 리소스 서버 엔드포인트로 API 요청
  - implict
    - 모바일 네이티브 앱 및 브라우저 기반 JS 애플리케이션(SPA)에 적합한 방식이며 응답 시 token을 직접 요청하고 반환한다.
    - 따라서, 절대 노출하면 안되는 client_secret, refresh_token은 사용되지 않는다.
    - 대신 access_token 만료기간을 늘리고 만료 시에는 사용자 인증을 반복한다.
    - 프로세스
      1. 사용자가 권한 위임에 동의(로그인) 후 인증 서버에서 리다이렉트 페이지로 access_token 획등
      2. access_token으로 리소스 서버 엔드포인트로 API 요청
  - password
    - 클라이언트가 사용자 인증 정보를 알고있는 경우에 적합한 방식이며 클라이언트가 매우 신뢰되어야 한다.
    - 인증 서버에 토큰 요청 시 바로 access_token을 획득한다.
  - client credentails
    - [//]: # (TODO:)
> [참고] [jsononject님 블로그](https://jsonobject.tistory.com/369)



## 키클록(Keycloak)
인증 솔루션 및 SSO 인증 지원
- 구성
  - 보호 자원
  - 자원 소유자
  - 애플리케이션
  - 인증 및 인가 서버

## 구성하기
### 한 개의 엔드포인트 보호
- 구현방법
  - 서비스 도커에 키클록 추가
    - 도커 설정 파일에 키클록을 추가하고 기존 서비스를 호출하던 포트로 키클록 매핑  
    - docker-compose.yml
    `docker-compose -f docker/docker-compose.yml up`
  - 키클록 설정
    - realm(영역) 추가
    - client application 등록(client, role, credential) - 관리자 또는 회원가입 없는 경우 user 까지 등록
- 프로세스
  1. 사용자 정보로 토큰 요청(grant_type: password)
  2. 발급된 access token(JWT)에서 사용자 정보 조회

### 조직 서비스 보호
- 스프링 시큐리티와 키클록 스프링 부트 어댑터로 자원 보호
- 엑세스 토큰 생성 및 관리는 키클록 서버의 책임
- 사용자 역할 별 권한 여부는 서비스 별로 정해지므로 보호 자원에 대한 설정은 서비스에서 필요. 어떻게?
    - 보호 서비스에 적절한 스프링 시큐리티와 키클록 JAR 추가
    - 키클록 서버 접속 구성 정보 설정
    - 서비스 엑세스 대상 및 사용자 정의
- 구현방법
  - 의존성 추가(키클록, 스프링 시큐리티)
  - 서비스 인증 구성요소 정의(각 properties)
  - 인가 대상 정의(SecurityConfig.java, @RolesAllowed)
  - 엑세스 토큰 전파
    - 게이트웨이
      - 기본적으로 민감 HTTP 헤더는 서비스로 전달하지 않음(Cookie, Authorization)
      - 필터 추가(gateway-server.yml, "RemoveRequestHeader = Cookie, Set-Cookie")
    - 전파받을 서비스
      - 앞서 서비스에 설정한 구현방법 3가지 적용
    - 요청 헤더 수정(SecurityConfig.java, KeycloakRestTemplate)
  - JWT 사용자 정의 필드 파싱 => 왜 gateway에서 파싱?
    - 의존성 추가
    - 파싱 

## 키워드
- 공개 API/ 비공개 API
  - 왜?



