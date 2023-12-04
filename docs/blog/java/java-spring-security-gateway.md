---
layout: default
title: "[Java] Spring Security - Gateway"
grand_parent: Blog
parent: java
permalink: /docs/blog/java/java-spring-security-gateway
nav_order: 1
date: 2023-02-13 10:04
---

## 스프링 시큐리티 Gateway

### 개념
- GlobalFilter
  - CustomFilter와 달리 모든 route에 대해 공통 적용되며 가장 먼저 실행되고, 가장 마지막에 종료된다.
- 