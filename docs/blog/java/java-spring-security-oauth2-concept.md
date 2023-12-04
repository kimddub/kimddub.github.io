---
layout: default
title: "[Java] Spring Security - 로그인 + OAuth2"
grand_parent: Blog
parent: java
permalink: /docs/blog/java/java-spring-security-oauth2-login
nav_order: 1
date: 2023-02-13 10:04
---

## 스프링 시큐리티 인증 구현 - 로그인
- - -
### 기본 클래스
- UserDetails: 사용자 정보 인터페이스
- UserDetailsService: 사용자 정보 전달 인터페이스
- AuthenticationProvider: 

### 커스텀
위 기본 클래스들을 구현하여 커스텀 할 수 있으며 다음 항목들을 커스텀한다.
- 로그인 액션(JWT 생성 + Cookie에 토큰 설정(세션 비활성화) + 리디렉션)
- 사용자(롤, 권한)
- 
### 로그인 액션 커스텀
```java
@Configuration
//@EnableWebSecurity // Boot에서는 WebSecurityEnablerConfiguration에서 자동 Import
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests((authorize) -> authorize
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                    .authorizationEndpoint(authorization -> authorization
                        .baseUri("/oauth2/authorization")
                        .authorizationRequestRepository(this.authorizationRequestRepository())
                    )
//                .redirectionEndpoint(redirection -> redirection
//                    .baseUri("/*/oauth2/code/*")
//                )
//                .userInfoEndpoint(userInfo -> userInfo
//                    .userService(this.oauth2UserService())
//                )
                    .successHandler(new OAuth2LoginSuccessHandler())
                    .failureHandler(new OAuth2LoginFailureHandler())
            );

        return http.build();

    }
}
```
- CSRF: REST API는 무상태성으로 CSRF 방어가 필요없다고 하나 토큰을 쿠키에 저장한다면 `HttpOnly` 설정으로 방어 필요(Auth 헤더는 CSRF로 부터 안전하나 클라이언트가 토큰 보관하기 위해 세션/쿠키 활용 필요) 

### 사용자 커스텀
```java
public class CustomUserDetails implements UserDetails{
	
	private String ID;
	private String PASSWORD;
	private String NAME;
	private String AUTHORITY;
	private boolean ENABLED;
	
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		ArrayList<GrantedAuthority> auth = new ArrayList<GrantedAuthority>();
		auth.add(new SimpleGrantedAuthority(AUTHORITY));
		return auth;
	}
    
    // ...

}
```



## 스프링 시큐리티 인증 구현 - OAuth2
- - -
### Registration Provider 설정
인가서버가 클라이언트에게 권한 부여하기 위해 리소스 소유자 인증&인가 정책 설정이 필요하다.
시큐리티 의존성 추가 후  구성 파일에 oauth2 정보를 설정하면 로그인 화면이 연동된다.
```yaml
# application.yml
spring:
  security:
    oauth2:
      client:
        registration:
          # 클라이언트 정보
        provider:
          # 인증서버 정보
```
(참고) ClientRegistration Config를 통해서도 인증 서버를 연동할 수 있다. Issuer 경로에서 메타정보를 통해 연동 설정을 자동으로 등록한다.
```java
@Configuration
public class SecurityConfig {

    // ...

    @Bean
    public ClientRegistrationRepository clientRegistrationRepository() {
        return new InMemoryClientRegistrationRepository(this.nxasClientRegistration());
    }

    private ClientRegistration nxasClientRegistration() {

        // OAuth2 Registration 등록없이 Provider 메타정보로 연동
        return ClientRegistrations.fromIssuerLocation("인증서버경로")
            .registrationId("nxas")
            .clientId("클라이언트ID")
            .redirectUri("리다이렉트URL")
            .build();
    }
}
```

### SecurityFilterChain 커스텀 - OAuth2Login
SecurityConfig 설정에서 로그인 핸들러를 커스텀한다.(리디렉션, jwt 생성, 쿠키 설정 등)
```java
@Configuration
public class SecurityConfig {


    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests((authorize) -> authorize
                .anyRequest().authenticated()
            )
            .oauth2Login(oauth2 -> oauth2
                    .authorizationEndpoint(authorization -> authorization
                        .baseUri("/oauth2/authorization")
                        .authorizationRequestRepository(this.authorizationRequestRepository())
                    )
//                .redirectionEndpoint(redirection -> redirection
//                    .baseUri("/*/oauth2/code/*")
//                )
//                .userInfoEndpoint(userInfo -> userInfo
//                    .userService(this.oauth2UserService())
//                )
                    .successHandler(new OAuth2LoginSuccessHandler())
                    .failureHandler(new OAuth2LoginFailureHandler())
            );

        return http.build();

    }

    @Bean
    public AuthorizationRequestRepository<OAuth2AuthorizationRequest> authorizationRequestRepository() {
      return new HttpSessionOAuth2AuthorizationRequestRepository();
    }
}
```

### SecurityFilterChain 커스텀 - OAuth2Client
최종 사용자 인증 부분을 커스텀하기 위해서는 OAuth2Client 메서드를 사용하여 인가까지만 자동 진행한다.   
OAuth2Login 메서드는 클아이언트 인증까지 자동으로 진행된다.
- OAuth2AuthorizedClient: 인가받은 클라이언트

### SecurityFilterChain 커스텀 - AuthorizationServer
엔드포인트와 인증을 위한 시큐리티 설정을 구성한다.

### OAuth2User 유저 객체
인증 완료 후 저장된 인증 정보 객체를 얻는 방법은 다음과 같다.
```java
public class AuthController {

    @GetMapping(value = "/user")
    public OAuth2User user(Authentication authentication) {

        OAuth2AuthenticationToken authenticationToken = (OAuth2AuthenticationToken) authentication;
//        OAuth2AuthenticationToken authenticationToken2 = (OAuth2AuthenticationToken) SecurityContextHolder.getContext().getAuthentication();

        OAuth2User oAuth2User = (OAuth2User) authenticationToken.getPrincipal();

        return oAuth2User;
    }

    @GetMapping(value = "/oAuth2User")
    public OAuth2User oAuth2User(@AuthenticationPrincipal OAuth2User oAuth2User) {
        return oAuth2User;
    }
}
```

### 로그인 성공 핸들러
핸들러를 통해 결과 처리 커스텀
```java
// 로그인 핸들러 - jwt 생성, 쿠키에 세팅, 리디렉션
```

## 리소스 서버 인가 로직 구현
- - -
실무에서는 인증 서버와 리소스 서버를 통일하여 시큐리티가 복잡하였는데  
본 포스팅에서는 분리하여 설명 할 수 있도록 작성한다.


## 클라이언트 구현
- - -
### 쿠키 설정
백엔드에서 인증 완료 후 응답 헤더의 쿠키에 토큰을 설정해서 리디렉션 했다면,  
클라이언트에서는 쿠키를 유지하여 이후 요청 마다 토큰을 전송하게 된다.
```typescript
export const serviceAxios = axios.create({
  baseURL: process.env.API_URL,
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json'
  },
  withCredentials: true // 쿠키 유지
});
```