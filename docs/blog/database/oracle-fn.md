---
layout: default
title: [Oracle] Function
grand_parent: Blog
parent: database
permalink: /docs/blog/database/oracle-fn
date: 2023-02-13 10:40
---

Function
===

## 함수
- - -
함수의 경우 프로시저와 달리 반환값이 있음


## 문법
- - -
먼저 파라미터가 없는 기본적인 생성문을 보자.
```sql
-- 파라미터(IN)가 없는 생성문
CREATE OR REPLACE FUNCTION {함수명}
  RETURN VARCHAR
IS
  {변수명} VARCHAR(100); -- 선언부
BEGIN

  {변수명} := 'returnValue'; -- 실행부
RETURN {변수명};

END;

-- 실행문
SELECT {함수명}() FROM dual; -- 반환값: returnValue
```
다음으로 파라미터와 리턴값, 예외처리를 포함한 생성문을 보자.  
예외처리는 SELECT 결과 ROW가 존재하지 않을 경우 NULL을 리턴하는 케이스를 정의한다.
```sql
-- 파라미터(IN)가 있는 생성문
CREATE OR REPLACE FUNCTION {함수명}({파라미터} varchar)
  RETURN VARCHAR
IS
  {변수명} VARCHAR(100); -- 선언부
BEGIN

  SELECT {컬럼} INTO {변수명} -- 실행부
  FROM {테이블} 
  WHERE {파라미터 관련 조건문}
RETURN {변수명};

EXCEPTION -- 예외처리부
  WHEN NO_DATA_FOUND THEN -- 모든 예외를 대체하려면 WHEN OTHERS THEN 사용  
  dbms_output.put_line('exception occurred! (' || sqlcode || ') : ' || sqlerrm);
RETURN '';

END;
```



  *참고 : [lee-mandu님 블로그](https://lee-mandu.tistory.com/59)*