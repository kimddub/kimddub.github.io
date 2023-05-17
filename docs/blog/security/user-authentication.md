---
layout: default
title: "[Security] 사용자 인증(Authentication)"
grand_parent: Blog
parent: java
permalink: /docs/blog/java/user-authentication
nav_order: 1
date: 2023-05-16 10:04
---

## 사용자 인증(Authentication)
- 사용자 인증이란, 서비스를 이용할 사용자 본인이 맞는지 확인하는 절차이다.
- 암호를 확인하거나 API를 호출한 대상이 누구인지를 인증한다.
- 유사한 용어로 인가(Authorization)란, API를 호출한 대상이 해당 데이터에 접근할 권한이 있는지 확인하는 절차이다.

### 필요한 이유
SPA(Single Page Application) 방식으로 구현된 프로젝트가 아니라면?
즉, MPA(Multi Page Application) 방식의 경우 SSR(Server Side Rendering)을 통해 페이지가 모두 랜더링되어 응답된다.
로그인한 사용자 정보를 서버 단에서 세션에 저장하고 있으며 타인이 API를 호출하여도 로그인 사용자를 식별 가능하다.

반대로 CSR(Client Side Rendering)은 별도의 API 서버 단을 두어 데이터를 호출하게 된다.
이 과정에서 로그인 한 유저가 아니더라도 타인이 민감한 데이터를 삭제하는 API 호출도 가능해진다는 것이다.

따라서 REST API 구현에 있어 어떤 대상이 API를 호출하고 있는지에 대한 '인증'이 중요해진다.

### 종류
- API Key
  - 대상마다 Key를 발급하여 저장하고, API 요청 시 마다 Key가 일치할 경우 해당 사용자임을 인증한다.
  - 단, Key가 탈취된 경우 이를 제어할 수 없다.
- API Token
  - 로그인 시 Token을 발급하고 만료 전까지 API 요청 시 Token과 일치하는 사용자임을 인증한다.
  - 탈취를 예방하여 accessToken 만료 주기를 짧게 설정하고, 재발급을 위한 refreshToken을 관리한다.

### Token 적용하기
  - accessToken은 로컬 변수에 저장하여 API 요청, refreshToken은 쿠키에 저장하여 보관하는 방법이 최선인 것 같다.
  - 클라이언트 단의 refreshToken 보관(localStorage, cookie)에 대한 위험 사례는 모두 존재하나 
  - 쿠키에 저장 시 httpOnly 옵션을 통해 XSS 공격 방어가 가능하고, RRT(Rotation Refresh Token) 방식 이용 시 탈취 피해를 최소화 할 수는 있다.
> 참고:   
> [kmlee95님 블로그](https://velog.io/@kmlee95/JWT%EC%9D%98-%EC%95%88%EC%A0%84%ED%95%9C-%EC%A0%80%EC%9E%A5%EC%86%8C)  
> [apparatus1님 블로그](https://velog.io/@apparatus1/JWT%EA%B0%80-%ED%83%88%EC%B7%A8%EB%90%98%EB%A9%B4-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%90%A0%EA%B9%8C)  
  - JWT 도입 시 토큰 자체에서 만료시간 관리가 가능하다.
  - 단, 이때도 탈취 당한다면 어쩔수 없다(?)
  - 최종적으로 서버 단에서 만료시간을 관리하는 것이고 TTL(Time To Live)을 지정한 NoSQL 사용을 고려할 필요가 있다.
> 참고: [junho5336님 블로그](https://velog.io/@junho5336/%ED%86%A0%ED%81%B0-%ED%83%88%EC%B7%A8-%EA%B3%A0%EB%A0%A4%ED%95%98%EA%B8%B0-Refresh-Token)    
  - 추가적으로 Spring Security OAuth Framework를 활용하면, ???