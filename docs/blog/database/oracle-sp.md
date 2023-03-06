---
layout: default
title: "[Oracle] SP(Stored Procedure)"
grand_parent: Blog
parent: database
permalink: /docs/blog/database/oracle-sp
nav_order: 4
date: 2023-02-13 10:40
---

SP(Stored Procedure)
===

## Procedure
- - -
> 프로시저란 SQL server에서 제공하는 프로그래밍 기능.
메서드 형식의 쿼리문을 작성하여 동작을 일괄처리하는 용도로 사용.
여러개의 쿼리를 사용해야 할 때 저장된 프로시저를 호출하여 프로그래밍.

### 장단점
- 장점
  - 여러 SQL문을 한번의 요청으로 처리
  - 네트워크 소요시간 감소
  - 데이터 관련 처리만 SP로 API 제공/호출하는 등의 애플리케이션과 구분하여 개발 가능
- 단점
  - 처리 성능 저하(문자나 숫자 연산에서는 C, JAVA 보다 성능이 낮음)
  - 디버깅 어려움
  - DB 확장 어려움

## 문법
- - -
### 프로시저 생성문
먼저 파라미터를 사용하지 않는 기본 프로시저의 생성 문법을 보자.
```sql
CREATE OR REPLACE PROCEDURE {프로시저명}
IS
  -- 선언부
  {변수명} NUMBER(10) := 10;
BEGIN
  -- 실행부
  DBMS_OUTPUT.PUT_LINE('변수값: ' || {변수명});
EXCEPTION
  -- 예외처리부
END;
```
생성한 프로시저를 실행시켜 보자.
```bash
$ SET SERVEROUTPUT ON;
$ EXECUTE {프로시저명};

변수값: 10
```
### IN 파라미터 프로시저 생성문
파라미터를 사용한 프로시저 생성 문법을 보자.  
파라미터 타입 앞에 `IN`을 작성하면 프로시저 실행에 필요한 값을 직접 입력받는 형식의 파라미터 지정이 가능하다.
```sql
CREATE OR REPLACE PROCEDURE {프로시저명} ({변수명} IN NUMBER)
IS

BEGIN
  DBMS_OUTPUT.PUT_LINE('변수값: ' || {변수명});
 
END;
```
생성한 프로시저를 실행시켜 보자.
```bash
$ BEGIN 
    프로시저명(1);
  END;

변수값: 1
```
### OUT 파라미터 프로시저 생성문
파라미터 타입 앞에 `OUT`을 작성하면 프로시저 실행 후 호출 프로그램으로 값을 반환받는 것이 가능하다.
```sql
CREATE OR REPLACE PROCEDURE {프로시저명} ({변수명} OUT NUMBER)
IS

BEGIN
  SELECT {컬럼명} INTO {변수명}
  FORM {테이블}
  WHERE {값이 10인 row를 찾는 조건문}
 
END {프로시저명};
```
생성한 프로시저를 실행시켜 보자.
```bash
$ DECLARE
    {변수명} NUMBER
  BEGIN
    {프로시저명}({변수명})
    DBMS_OUTPUT.PUT_LINE('변수값: ' || {변수명});
  END;

변수값: 10
```

  *참고 : [pingfanzhilu님 블로그](https://pingfanzhilu.tistory.com/entry/Oracle-%EC%98%A4%EB%9D%BC%ED%81%B4-PLSQL-%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80procedure-IN-OUT-%EC%82%AC%EC%9A%A9%EB%B2%95)*