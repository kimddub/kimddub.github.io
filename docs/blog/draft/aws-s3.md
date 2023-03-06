---
layout: default
title: java
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/java
nav_order: 3
---

AWS s3
===========
# aws cli 설치

# aws key 등록
```
$ aws configure
```

# aws 디렉토리 내 모든 파일(디렉토리) 복사
```
$ aws s3 cp . s3://[버킷]/[디렛토리] --recursive
```
