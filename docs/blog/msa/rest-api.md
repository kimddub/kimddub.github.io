---
layout: default
title: "서비스 간 통신"
grand_parent: Blog
parent: msa
permalink: /docs/blog/msa/feign-client
nav_order: 1
date: 2023-07-07 10:00
---

# 서비스 간 통신
기존에 REST API를 개발하면서 사용했던 통신 방법은 HttpClient나 RestTemplate이었다.
MSA를 공부하면서 알게된 Feign Client는 구현 방법도 간단하며 다양한 장점들이 있어 도입해보기로 했다.

## Feign Client
- - -
### 키워드
- @EnableFeignClients
  - 부트클래스(@SpringBootApplication) 파일에 작성하지 않는다.
- 설정
  - 각종 타임아웃, 로깅 등의 설정은 config 또는 yml에서 적용할 수 있다.
- @Configuration
  - 해당 어노테이션 적용 시 모든 클래스에 반영되므로 공통 구성만 적용
  - 이외 Client 파일에서 configuration 속성 사용
- HeaderConfiguration
- ErrorDecoder