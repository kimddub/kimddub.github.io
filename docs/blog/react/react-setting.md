---
layout: default
title: "[React] 2.세팅하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/setting
nav_order: 3
date: 2023-04-07 10:04
---

# 세팅하기
리액트 프로젝트를 세팅할 떄, 기본적으로 구성해야할 사항들을 정리한다.

## 프로젝트 구조
- - - 
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

## 구성정보 및 환경변수 관리
- - - 
`config.js`와 `.env` 모두 환경에 따른 민감 정보를 관리할 수 있지만, 형상관리 서버 노출에 주의해야 한다.
### config.js 파일
`Locale` 설정과 같이 글로벌 변수이지만 사용자가 시스템 설정을 통해 변경의 여지가 있을 때 사용한다. 
### .env 파일
create-react-app을 통해 제작한 앱의 경우에는 .env을 지원하며, 환경에 따라 구성값 변경의 여지가 있을 때 환경 별로 구성정보를 설정한다.
- .env가 항상 불러오는 전체 용도
- .env.development는 npm start를 통해 실행할 경우 불러오는 경우
- .env.production는 npm run build를 실행할 경우 빌드 타임에 env를 가져오는 방식


## 라우터
- - - 
- 라우팅
- 템플릿
- 예외처리


