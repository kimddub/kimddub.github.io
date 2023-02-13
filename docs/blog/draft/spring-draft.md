---
layout: default
title: angular
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/angular
nav_order: 3
---

spring boot
===========

# JPA
angular 게시판 구현을 위한 RESTful API 서버 구축.
Spring boot + Gradle + JPA + PostgreSQL

## 	context.xml
<WatchedResource> 태그: 설정 파일의 변경이 있을시 리로드
<Resource> 태그: JNDI resource 선언부
JNDI(Java Naming and Directory Interface): 
    디렉터리 서비스에서 제공하는 데이터 및 객체를 discover, lookup 하기 위한 java api.
    데이터베이스의 DB Pool을 미리 네이밍하는 방법으로 사용하며 WAS에 JNDI를 설정하면 웹 어플리케이션 단에서의 호출이 간단해짐.
    