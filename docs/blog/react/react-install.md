---
layout: default
title: "[React] 1.설치하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/install
nav_order: 1
date: 2023-04-07 10:04
---

## 설치하기

### 스펙
  - Node.js
    - 18.15.0(LTS)

### 사전준비
1. Node.js 설치(https://nodejs.org/en)  
    리액트 프로젝트 구축 시, JSX 문법으로 작성된 컴포넌트 파일들을 한개로 결합하는 도구(Webpack)와 새로운 js 문법들을 사용하기 위한 도구(Babel) 등이 Node.js 기반으로 만들어짐.
   (결론: [Create React App](https://create-react-app.dev/docs/getting-started) 사용하기 위해 설치)  
    - babel
      javascript compiler(바이너리 파일로 변환하는 것은 X). React JSX 문법을 Vanilla javascript로 변환 및 최신버전 문법 변환 등.
    - webpack
      Javascript Application 모든 코드들을 합치고 브라우저 이해 형태로 변환. 파일들을 transpile하는 build system임. 
    - CRA(Create React App)
      React 개발환경 설정 시 필요한 위 2가지 도구를 포함하여 간단하게 세팅해주는 패키지
    - 환경변수 설정 후 재부팅(intellij 터미널에서 사용하기 위함)
      - ![ev nodejs](https://user-images.githubusercontent.com/46553770/230529139-29c8aafa-dbfd-45e5-a6e8-ac225a7e95be.png)
2. yarn 설치
    ![install yarn](https://user-images.githubusercontent.com/46553770/230531382-3de131a1-6c19-4a49-892f-130278d491cd.png)     
    ```shell
    npm install --gelobal yarn
    yarn --version
    # 이때, 보안 오류 발생할 경우 PowerShell 관리자 권한으로 열고 아래 명령어 실행 
    Set-ExecutionPolicy RemoteSigned 
    ```

### 프로젝트 생성
- 터미널에서 다음 명령어 수행
```shell
# 프로젝트 생성할 상위 경로에서 CRA 실행 
npx create-react-app {경로 및 프로젝트명}

# 리액트 문법의 불변성 보존을 위한 라이브러리 설치
yarn add immer
```
- Intellij GUI 버전으로 프로젝트 생성
  - ![Intellij New Project](https://user-images.githubusercontent.com/46553770/230525374-b9c40641-8a7a-448a-9401-a806d66c9b5f.png)

### 프로젝트 배포