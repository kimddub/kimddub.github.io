---
layout: default
title: "[React] 3.문법"
grand_parent: Blog
parent: syntax
permalink: /docs/blog/react/syntax
nav_order: 3
date: 2023-04-07 10:04
---

## 문법
기초적인 문법은 명강의([코딩애플](https://www.youtube.com/watch?v=LclObYwGj90&list=PLfLgtT94nNq1e6tr4sm2eH6ZZC2jcqGOy))를 따라가면 신속하게 맛볼 수 있다.
이외 기록할 문법이나 규칙들을 작성해본다.

### Data Binding
  js 단에서 HTML을 쉽게 리턴할 수 있다.
  이때, 데이터 바인딩도 굉장히 쉽게 하는데 다음과 같은 문법을 사용한다. (변수, 함수, 클래스명 모두 가능)
  ```js
    <div className={ divClassName }> { paramName } </div>
  ```

### JSX
- style
  데이터 바인딩에 뛰어나다 보니 js에서 기본적으로 사용하는 기호들은 HTML 문법과 함께 쓸 수 없다.
  이때, element 속성으로 style을 쓸 때 문법이 다음과 같이 달라진다. (객체 타입처럼 사용)
  ```js
    <!-- 기존 html 문법 -->
    <div style="width: 100px; font-size: 10px;"></div>
    <!-- react 문법 -->
    <div style={ { width : '100px', fontSize: '10px'} } ></div>
  ```
  
### State
- 변수
state는 데이터 저장소와도 같다. 
변수에 할당한 데이터가 변경되면 HTML을 새로고침해야 하는 반면, state에 담은 변수는 데이터가 변경되면 자동으로 재랜더링된다.   
따라서, 자주 바뀌거나 중요한 변수는 state에 담아서 저장하도록 한다.
  ```js
    let [data, dataChangeFn] = useStat(['char1', 'char2']);
  ```
- 변수 변경 함수
state에 담은 데이터는 해당 변수를 직접 수정하면 HTML 재랜더링이 동작하지 않는다.
따라서 변경 함수로 변수를 변경한다.
  ```js
  let [int, intChangeFn] = useStat(0);
  intChangeFn(int + 1);
  console.log(int);
  <!-- 출력값: 1 -->
  ```
  변경 함수의 인자는 변수를 그대로 대치한다고 생가하면 된다. 따라서, 배열의 경우 기존 배열 구조를 그대로 변경해줘야 한다.
  ```js
  let [data, dataChangeFn] = useStat(['char1', 'char2']);
  dataChangeFn(['new char1', 'char2']);
  console.log(data[0]);
  <!-- 출력값: new char1 --> 
  ```
  이럴 경우에 배열이나 객체의 경우, 일부 필드만 값을 변경하게 될 때 나머지 필드들은 기존 값 복사가 필요하다.  
  이때, 변수를 그대로 `=` 연산자로 대입할 경우 변수값을 공유할 뿐이기 때문에 변수를 직접 바꾼 행위와 같다.  
  HTML 재랜더링이 동작하지 않기 때문에 `deep copy` 를 통해 복사한다.
  ```js
  let [data, dataChangeFn] = useStat(['char1', 'char2']);
  var newData = [...data];
  newData[0] = 'new char1';
  dataChangeFn(newData);
  console.log(data[0]);
  <!-- 출력값: new char1 --> 
  ```