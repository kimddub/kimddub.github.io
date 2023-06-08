---
layout: default
title: "[React] 2.세팅하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/setting
nav_order: 3
date: 2023-04-07 10:04
---

## 세팅하기
리액트 프로젝트를 세팅할 떄, 기본적으로 구성해야할 사항들을 정리한다. 

### 프로젝트 구조
```
project
├─ src // 컴포넌트 상위 경로
│  ├─ assets // 이미지 리소스 등
│  ├─ components // 컴포넌트
│  ├─ config // 설정
│  ├─ constans
│  ├─ context // global 변수를 사용하기 위해 provider, reducer 정의
│  ├─ hooks // custom hooks
│  ├─ pages // router pages
│  ├─ services // business api 
│  ├─ styles // css, scss
│  └─ util 
├─ node_modules // 라이브러리 상위 경로
├─ public // static 파일 상위 경로
└─ package.json // 라이브러리 명 목록 작성
```

### 라우터
- 라우팅
- 템플릿
- 예외처리

