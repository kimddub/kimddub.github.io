---
layout: default
title: typescript
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/typescript
nav_order: 3
---

Typescript
===========

document : https://typescript-kr.github.io/(https://typescript-kr.github.io/)


# Node
Node.js는 웹브라우저에 종속적이던 JS를 OS환경에서 실행 가능하도록 Chrome V8 엔진 제공.
프론트에서 `<script></script>` 태그로 임포트 모듈을 통해 JS를 불러왔다면 Node.js에서는 선언된 모듈에 대해 코드처럼 참조가 가능.

# Typescript
JS는 타입, 프로퍼티 등을 선언하고 사용하는데에 엄격하지 않음.
이에 발생하는 오류들을 Typescript는 프로그램을 실행시키기 전 검사 및 표출함.

# 환경세팅
- node.js 설치
- typescript 설치
```
# typescript 설치
npm install -g typescript

# project workspace 경로 이동
npm init
tsc -init

# package.json, tsconfig.json 생성됨

# 나의 경우 vscode terminal에서 보안오류 발생하면 관리자 실행 후 아래 명령어
Set-ExecutionPolicy RemoteSigned
```
- .ts 파일 작성
- .ts 컴파일
```
# ts 컴파일
tsc {파일명}

# ts 자동컴파일
tsc -w
```
- vscode runnable env for test
  - js 실행 : Ctrl + Alt + N
  - server 실행방법 참조(https://webnautes.tistory.com/1473)




