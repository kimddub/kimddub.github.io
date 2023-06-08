---
layout: default
title: "[React] 0.시작하기"
grand_parent: Blog
parent: react
permalink: /docs/blog/react/start
nav_order: 1
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
### Data Flow
단방향 데이터 흐름으로 Angular.js(양방향 데이터 바인딩) 보다 데이터 흐름 추적이 용이. 
### Component 구조
컴포넌트 단위로 분리된 View를 조립하여 화면을 구성. 코드 파악이 용이하고 재사용성 높음.
### Virtual Dom
최소한의 변경사항만 DOM에 반영하여 효율성과 속도 개선.
###`Props and State
읽기 전용 데이터인 Props와 동적 데이터인 State.
### JSX
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
    - 함수형 컴포넌트가 나오고 나서 사용할 일이 거의 없지만,  
    함수형 컴포넌트 + Hooks 로 못하는 작업이나 라우터 라이브러리(react-native-navigation)에서 사용할 일이 종종 있다고 함.
    - 생명주기 메서드(Lifecycle Method)가 이에 속함.
    - 생명주기 메서드 중 대체 불가한 메서드 
      - getSnapshotBeforeUpdate: state 변화로 리랜더링이 될 때, 리랜더링 직전 DOM 값을 리턴하는 함수.
      - componentDidCatch: 예외처리 하지 않은 에러 발생 시 인지하도록 하는 함수.
  - [//]: # (TODO:)
- Hooks  
  후킹이라는 기본적으로 호출을 가로채는 이벤트성 기능을 한다. 다음과 같은 기술들이 포함된다.
  - useState  
  실시간 렌더링이 감지되는 변수와 값 변경 함수를 지정할 수 있음.
  - useReducer   
    useState와 같은 상태 관리 함수이나 컴포넌트 외부에서 사용 가능하고, 여러 상태를 관리할 수 있음.    일반적으로 useState를 사용하다가 한 함수 내에서 setter 사용 빈도가 잦아지면 useReducer 사용을 고려할 것.
    - context API: 최하위 컴포넌트에서도 dispatch를 바로 사용 가능하여 props 전달을 최소화 가능함.
  - useRef  
    DOM element를 선택 할 수 있음. 리렌더링되지 않아도 상태를 바꾼 변수를 바로 조회 할 수 있음.
  - useEffect  
    컴포넌트가 마운트/언마운트 될 때 특정 작업을 처리할 수 있음.
      - mount: 컴포넌트가 표출되는 행위. props 값 설정, API 요청, 라이브러리 사용, 반복작업 또는 작업 예약.
      - unmount: 컴포넌트가 사라지는 행위. 등록된 작업(serInterval, setTimeout) clear, 라이브러리 인스턴스 제거.
      - 특정 작업을 위해 리스닝하는 변수를 deps 배열에 포함시킬 것.
  - useMemo  
    연산된 값을 재사용할 수 있어 성능 최적화가 가능함. 
    변경 Hook으로 인해 재랜더링 될 때 컴포넌트 내의 불필요한 랜더링이 발생하면 자원 낭비가 발생.
    재사용하려는 변수를 정의하는 함수 내에 사용되는 변수를 deps 배열에 포함시킬 것.
  - useCallback  
    useMemo 와 같은 용도로 사용하나 특정 값이 아닌, 특정 함수를 재사용 할 때 사용.
    재사용하려는 함수에 사용되는 변수를 deps 배열에 포함시킬 것.
    반대로, React.memo 적용 컴포넌트라면 deps의 변수를 모두 빼주어야 불필요한 랜더링을 피할 수 있음. 
  - [//]: # (TODO:)
- 생명주기 메서드(Lifecycle Method)
  - 컴포넌트가 브라우저 표출 관련 동작을 할 때 호출되는 메서드.
  - 클래스형 컴포넌트에서만 사용 가능.
  - useEffect와 유사한 역할. 동작 방식은 다름.
- 예외 처리
  - React에서 에러가 있는 경우 화면상에 흰 페이지만 표출되게 됨. 
  - 에러 발생을 방지하는 방법
    - null checking: props가 넘어오지 않아도 에러 발생 전에 특정 페이지 표출 등의 동작으로 오류 발생 전 방지가 가능함.
  - 에러 발생을 알려주는 방법
    - defaultProps 설정: 기본값을 설정하여 props가 넘어오지 않아도 오류 발생 대신 기본값을 통해 인지함.
    - PropTypes: props 설정이 없는 경우 개발 단계에서 경고 확인 가능함.
  - componentDidCatch 메서드로 에러 분기
    - 예외 처리하지 않은 에러가 발생했을 때 상황을 알려줌.
  - sentry 연동
    - Front-End 에러 발생 시 이를 감지, 수집, 모니터링하는 플랫폼 
    - 에러 발생 시 예외처리되지 못한 케이스를 원인 파악하기 까지의 시간 소요 및 에러 추적의 어려움 등이 있음
    - Front-End 발생 가능한 에러 종류
      - 데이터 영역 에러 - 처리 가능
      - 화면 영역 에러 - 처리 가능
      - 외부요인에 의한 에러 - 대처 불가능
      - 런타임 에러 - 대처 불가능
- CSS in JS
  - styled-components
    - Tagged Template Literal
    - Template Literal
      - javascript에서 문자열 내에 객체를 조회하고자 할 때 사용하는 문법으로 문자열과 변수(${val})를 그레이브(`)로 감싸면 변수가 값으로 대치된다.
  - emotion
  - styled-jsx
- Router
  - SPA에서도 페이지가 분리될 수 있는데, 이때 페이지로 요청하기 위한 URL을 매핑해주는 도구로써 사용
  - react-router
    - 써드파티 라이브러리
  - Next.js
    - 리액트 프레임워크로 CRA와 같이 리액트 환경 설정, 라우팅, 최적화, 서버사이드 랜더링 등 제공
- Code Spliting
  - 코드 스플리팅 VS 서버사이드 랜더링
  - 하나의 파일에 모든 로직이 들어가게 되면 용량 및 페이지 로딩 성능이 저하됨
  - 당장 필요한 코드가 아닌 경우 분리하여 상황에 맞게 로드하여 사용하는 방향으로 개발하는 작업
  - import 방식(promise 리턴), HoC 방식(withSpliting), 라우터 방식, react-loadable
- Redux 
  - 왜 사용하는지?
  - 사용방법: react-redux 설치
  - [//]: # (TODO:)
- 비동기 통신
  - 사용방법: react-thunk 라이브러리나 react-saga 라이브버리 설치
  - [//]: # (TODO:)
- styled-components
  - [//]: # (TODO:)