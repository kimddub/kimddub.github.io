---
layout: default
title: angular
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/angular
nav_order: 3
---

angular
===========

# Angular
one framework. server와 RESTful API 방식으로 통신.
client와 server를 분리함으로써 다양한 클라이언트 서비스(ios/ android/ 웹/ 관리자 웹 등)에 동일한 데이터를 CRUD하는 server를 하나로 구동할 수 있는 플랫폼이 된다.
SPA(single page Application)으로 내용이 변경되어 페이지 전체를 새로고침하지 않고 변경부분만 로드한다.

# 기동

- 패키지 설치
```
#augular CLI 설치
npm i @angular/cli -g

#choco 설치
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

#yarn 설치
choco install yarn
```
- 기동
```
#서비스 시작
yarn start
#또는
ng serve --o

#오류조치 : Cannot find module '@angular-devkit/build-angular/package.json'
npm install --save-dev @angular-devkit/build-angular
```

# 프로젝트 구조
```
/src┬ /app
    │   ├ app.component.ts : app-root component 정의
    │   ├ app.module.ts : module 정의. module 내의 component를 모두 선언.
    │   └ app-routing.module.ts : module 별 path 정의
    ├ /{module name}
    │   ├ {module name}.module.ts : module 정의. module 내의 component를 모두 선언.
    │   └ /{function-name}
    │       └ {function-name}.component.ts : 기능별 component 정의
    │
    └ /environments
        ├ environment.prod.ts
```
# 프로젝트 기본
- concept(https://docs.angularjs.org/guide/concepts)
- @Component : Component decorator는 작성하는 class를 컴포넌트로 동작하도록 정의하는 부분. spring의 annotation과 유사. import하여 사용 
    ```
    import { Component } from '@angular/core';
    
    // metadata
    @Component({ 
        selector: 'app-root', 
        templateUrl: './app.component.html', 
        styleUrls: ['./app.component.css'] 
    })
    ```
# 프로젝트 세팅
- 생성
```
ng new {프로젝트 명}
```
- ui 모듈 세팅
    ```
    # angular material(ui template) 설치
    npm install --save @angular/material @angular/cdk @angular/animations 
    # flexbox css layout(반응형 웹) 설치
    npm install @angular/flex-layout@latest --save

    # src/app/modules 디렉토리 생성 후  모듈 생성
    ng generate {모듈 명}
    # or
    ng g m {모듈 명}
    ```
    - icon(https://fonts.google.com/icons?selected=Material+Icons:delete_forever)

- 컴포넌트 세팅
```
# /src/app/component 디렉토리 생성 후 컴포넌트 생성
ng g c {컴포넌트 명} -- flat
# -- flat 은 디렉토리 미생성 옵션

# 서비스 생성
ng g s {서비스 명} 
```
- 라우팅 세팅


# 문법
- 템플릿
컴포넌트 클래스에서 정의된 변수 및 함수들은 html에서 아래의 코드로 작성된 템플릿으로  바인딩된다.
    - 문자열 바인딩 : {{}}
    - 프로퍼티 바인딩 : []
    - 이벤트 리스너 : ()
- 디렉티브
DOM 구조를 동적으로 변경할 수 있도록 하는 코드
    - *ngIf와 *ngFor



