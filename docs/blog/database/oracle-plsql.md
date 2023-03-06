---
layout: default
title: [Oracle] PL/SLQ(Procedural Language/SQL)
grand_parent: Blog
parent: database
permalink: /docs/blog/database/oracle-plsql
nav_order: 3
date: 2023-02-13 10:04
---

PL/SLQ(Procedural Language/SQL)
===

# PL/SQL 기초
> 주로 자료 내부에서 SQL 명령문만으로 처리하기에는 복잡한 자료의 저장이나 프로시저와 트리거 등을 작성하는 데 쓰인다. 
PL/SQL의 구조는 에이다 프로그래밍 언어를 본떠 만들어졌다고 알려졌다. 
따라서 두 언어는 그 구조가 범용 언어인 파스칼의 구문과 비슷하다. 
범용 언어인 C와 C++ 그리고 파스칼 및 포트란 등의 프로그래밍 언어와는 다른 점으로 
범용 언어들이 컴퓨터 시스템에서 특정한 작업을 처리하기 위해 만들어진 언어라고 볼 때 PL/SQL은 단지 오라클의 관계형 데이터베이스 (RDBMS)에서만 사용된다는 점이다.
- SQL에서 프로그래밍 언어의 역할을 수행한다.
- 변수를 만들고 반복문 등을 수행할 수 있다.
- 수행절차 
  1. PL/SQL 블럭 실행
  2. PL/SQL 엔진에서 PL/SQL 와 SQL 분리
  3. Database Server에서 SQL 처리
  4. 처리된 SQL 결과를 PL/SQL 엔진으로 전달
  5. PL/SQL 엔진에서 나머지 작업 수행

## 문법
- - -
### PL/SQL 블럭
```sql
DECLARE
    -- 선언
BEGIN
    -- 실행
EXCEPTION
    -- 예외처리
END;
```
- DECLARE(선언)
  - 변수 정의
- BEGIN(실행)
  - 실행될 PL/SQL 구문
- EXCEPTION(예외처리)
  - 예외 처리 구문
### SERVEROUTPUT
기본적으로 PL/SQL은 결과값을 출력하지 않는다.  
결과값을 확인하려면 다음과 같이 SERVEROUTPUT을 ON으로 설정해줘야 한다.
```sql
SET SERVEROUTPUT ON;
SET SERVEROUTPUT OFF;
```

### ERROR
PL/SQL은 오류를 따로 출력하지 않는다.
다음과 같이 오류를 확인할 수 있다.
```sql
SHOW ERRORS;
```


### 대입연산자(:=)
  - 오라클에서는 변수에 값을 대입하는 구문을 사용한다.
  - 이때, 대입연산자를 사용하며 equal(=) 기호가 아닌 대입연산자(:=) 기호를 사용한다. 

### [SP(Stored Procedure)](/docs/blog/database/oracle-sp)

### [Function](/docs/blog/database/oracle-fn)

### EXECUTE
프로시저를 실행할 때 사용. 
함수는 반환 값을 받으므로 `호출`이라고 표현하나 프로시저는 `실행`이라는 표현을 많이 씀.
프로시저의 경우 반환 값이 없으므로 SELECT 절에는 사용하지 않고 다음과 같이 실행한다.
```sql
EXECUTE {프로시저명} ({파라미터}); -- 또는 EXEC {프로시저명} ({파라미터}); 
```
프로시저의 파라미터가 많은 경우에는 `=>`을 사용하여 파라미터명을 값과 함께 입력하기도 한다.
```sql
EXECUTE {프로시저명} (
                    {파라미터1 명} => {파라미터1 값},
                    {파라미터2 명} => {파라미터2 값},
                    {파라미터3 명} => {파라미터3 값}
                    );   
```

### CALL
Oracle 9i부터 생긴 표준 SQL의 커맨드. 함수나 프로시저 호출에 사용.  
EXEC과 유사하나 실행계획이 다르며 성능이 더 좋다고 함.

### CURSOR
SQL 처리 결과 메모리를 가리키는 포인터로 여러 결과 ROW에 순차적으로 접근할 수 있다.  

- 종류
  - 묵시적 커서
    오라클 내부에서 자동 생성된 SQL 실행시마다 자동으로 생성/실행되는 커서
  - 명시적 커서
    사용자가 직접 정의해서 사용하는 커서
- 문법
  ```sql
  -- 선언
  CURSOR {커서명} ({매개변수})
  IS
    -- 실행문
  ;
  
  -- 열기
  OPEN {커서명} ({매개변수});
  
  -- 사용
  LOOP
  FETCH {커서명} INTO {변수명}
  EXIT WHEN {커서명}%NOTFOUND;
    -- 실행문;
  END LOOP;
  
  -- 닫기
  CLOSE {커서명};
  ```


*참고 : [cailisin님 블로그](https://cailisin.tistory.com/149)*
*참고 : [coding-factory님 블로그](https://coding-factory.tistory.com/455)*