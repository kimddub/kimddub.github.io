---
layout: default
title: "[React] 0.시작하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/start
nav_order: 0
date: 2023-04-07 10:04
---

## 리액트란
- SPA(Single Page Application) 웹 프레임워크.
  > SPA 프레임워크 특징으로는 DOM을 직접 건들면서 작업하여 코드가 난잡해지는 상황을 지양.  
  > 데이터가 변경되면 DOM 속성도 바뀌도록 업데이트 가능하도록 작업을 간소화해주는 식으로 웹 개발의 어려움을 해소시킴.  
  > 이때, 상태에 따라 DOM을 업데이트하는 규칙을 정의하는 관점과 달리 리액트는 모든걸 새로 보여주는 관점에서 차이가 있음.   
  > 다만, 모든 소스를 날리고 새로 보여주면 성능 저하가 우려되므로 [Virtual DOM](#virtual-dom)을 통해 이를 가능케 함.  
- js 라이브러이의 일종으로 사용자 인터페이스 구현이 목적.
- 개발사 Meta(facebook).

## 특징
  ### `Data Flow`
  단방향 데이터 흐름으로 Angular.js(양방향 데이터 바인딩) 보다 데이터 흐름 추적이 용이. 
  ### `Component 구조`
  컴포넌트 단위로 분리된 View를 조립하여 화면을 구성. 코드 파악이 용이하고 재사용성 높음.
  ### `Virtual Dom`
  최소한의 변경사항만 DOM에 반영하여 효율성과 속도 개선.
  ### `Props and State`
  읽기 전용 데이터인 Props와 동적 데이터인 State.
  ### `JSX`
  js를 확장한 문법.
> 출처: [jini_eum님 블로그](https://velog.io/@jini_eun/React-React.js%EB%9E%80-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC)

### 리액트와 함께 언급되는 주요 키워드들
- Context
  - 컴포넌트에서 데이터를 전달하는 Props가 아닌 또 다른 방식 중 하나
- Props Drilling
  - Props로만 데이터를 전달하면 발생할 수 있는 문제로, 데이터를 깊숙이 전달해야 하는 경우 여러 컴포넌트를 거쳐야 함
> 출처: [veloprt님 블로그](https://velog.io/@velopert/react-context-tutorial)  
- 컴포넌트
  - 함수형 컴포넌트
  - 클래스형 컴포넌트(old)
  - [//]: # (TODO:)
- Hooks
  - 왜 사용하는지?
  - useState, useRef 등의 방법이 있음
  - [//]: # (TODO:)
- Redux 
  - 왜 사용하는지?
  - 사용방법: react-redux 설치
  - [//]: # (TODO:)
- 비동기 통신
  - 사용방법: react-thunk 라이브러리나 react-saga 라이브버리 설치
  - [//]: # (TODO:)
- styled-components
  - [//]: # (TODO:)