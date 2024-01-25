---
layout: default
title: "[React] 4.배포하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/deploy
nav_order: 9
date: 2023-04-07 10:04
---

## 배포하기
대망의 배포하기 ㄷㄷ

### 배포 형식
리액트는 `npm build` 스크립트를 통해 build 디렉토리 하위에 정적 소스로 컴파일된다.
이 디렉토리 그대로 배포 경로에 두고 웹 서버로 서빙해주면 브라우저에서 접속 가능해진다.

### 빌드
node 패키지 관리 도구인 npm으로 로컬에서 또는 CI/CD 환경에서 빌드한다.

### 환경 변수 파일
리액트에서 기본적으로 `.env` 파일에 설정하지만 각 개발/운영 환경에 맞는 파일로 빌드하려면 `dotenv-cli` 패키지가 필요하다.
```shell
# 적용예시

# .env.development 파일 생성

# package.json
  "scripts": {
    "start": "set PORT=3000 && react-app-rewired start",
    "build": "react-app-rewired build",
    "build:development": "dotenv -e .env.development react-app-rewired build", # 해당 파일 설정
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
```

### 웹 서버
- nodejs 
  - 런타임 환경인 nodejs를 통해 띄우고 포팅 할 수도 있으나 결국 nginx는 필요하다고 한다.(자체 클러스터링을 통해 인스턴스를 분산하는 과정에서 로드 밸런싱이 필요한듯)
- nginx
  - 동적 콘텐츠가 필요한 것이 아니라면 단순 nginx로 포팅만 해주면 될듯 하다.

### CI/CD
Gitlab CI를 통해 개발서버에 빌드 파일을 전달하는 방식을 사용한다.
- 사전 작업
  - Docker Image 생성
  - .gitlab-ci.yml 빌드 스크립트 구성
- SE 작업
  - 빌드 후 rundeck job 호출 설정 
  - rundeck 배포 환경 구성
  - 배포 후 재시작 스크립트 설정
