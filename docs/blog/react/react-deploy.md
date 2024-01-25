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
보안상 빌드, 모듈 디렉토리는 형상관리에 푸시하지 않으므로 git에 있는 소스코드들을 개발서버에 빌드된 상태로 배포하려면
소스코드를 빌드 할 수 있는 node 환경이 필요하고 패키지 설치 및 빌드 작업하는 스크립트 설정이 필요하여  
하기와 같이 사전 작업을 진행한다.
- 사전 작업
  - Docker Image 생성
    - 배포하기 위한 소스코드 빌드 작업 환경을 도커 이미지로 설정
    - 이미지 생성을 위한 Dockerfile 작성 및 이미지 빌드 필요
  - `.gitlab-ci.yml` 설정
    - 빌드 환경에서 git 소스코드를 빌드하기 위한 스크립트 작성
    - `image`: 위에서 생성한 Docker image를 registry 업로드 후 경로 설정
    - `stages`: 실행된 도커 컨테이너 내부에 git 소스코드를 큰론해두는 경로(기본적으로 build)
    - `script`: 패키지 설치 및 모듈 빌드 스크립트 
  - 테스트
    - 빌드 환경은 도커로 구성하므로 로컬에서 ci 설정이 정상 작동하는지 테스트하고자 다음과 같이 진행했다. 이상없을 경우 해당 ci 설정으로 하기와 같이 배포 환경 구성을 진행한다.
    1. 도커 이미지 기반으로 컨테이너 실행
       ```shell
       docker run --rm -it --name ats-front -v /mnt/c/Users/kimddub:/mnt image_name /bin/sh
        ```
    2. 로컬 프로젝트 소스 WSL -> 도커 컨테이너 내부로 복사
    3. 스크립트에 작성된 명령어들 실행 및 확인
- SE 작업
  - 빌드 후 rundeck job 호출 설정 
  - rundeck 배포 환경 구성
  - 배포 후 재시작 스크립트 설정
