---
layout: default
title: draft-1
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/java
nav_order: 1
---

JAVA
===========

# Java11
- jdk 11 설치
- spring boot gradle 프로젝트를 아무리 clone해도 IDE에서 인식 못함(STS4, eclipse + STS plugin)
- java 11 plugin 설치? 목록에 없음
- 설정
    - preference > installed JRE's 버전
    - properties > java compiler 버전
    - properties > project facet 버전

## 용어
### WAS(Web Application Server)
웹 애플리케이션을 구동하는 서버. 단순히 서블릿 구동을 위해서만 사용되는 것은 아니며, 운영 및 관리, 장애 대응 등을 포함하는 복잡한 SW 시스템.

컴퓨터가 WAS로 동작하기 위해서는 JAVA EE 혹은 Servlet Container가 필요하다.
- JAVA EE를 지원하는 WAS : IBM, Oracle, JBoss, JEUS, Glassfish 등
- Servlet Container : Aparch Tomcat, Jetty, Undertow 등

*참고 : <https://dinfree.com/lecture/backend/javaweb_1.3.html>*

### WAR, JAR

### memory 관련
intellij low memory warning
heap memory, xms, xmx 
