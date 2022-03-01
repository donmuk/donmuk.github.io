---
layout: post
title: "[ORACLE] function 생성, 사용방법"
comments: true
description: ""
keywords: ""
tags: [DB]
---

예를 들어, 우리가 sql문을 짜다보면 날짜에 관해 많이 접근하게 된다.

근데 우리는 날짜를 

SELECT TO_CHAR(LIMIT_DATE, 'YYYY-MM-DD') AS deadline from example

YYYY-MM-DD와 같이 바꾸고 싶을 때 위와 같은 형태로 xml에 작성을 한다.

문제는, 우리는 매번 YYYY-MM-DD와 같은 형태로 만드려면 반복 작업을 귀찮게 해야만 한다..

그럴때 쓰는 것이 바로 함수 function.

 
function의 기본형을 살펴보자
```sql
CREATE OR REPLACE FUNCTION 함수 이름 
       (
           매개변수1, 
           매개변수2, 
           ...
       ) 
       RETURN 데이터타입
IS
    [
AS
    ] 변수, 
    상수 등 선언 
BEGIN 
    실행부 　 RETURN 반환값; 
    [
EXCEPTION 
    예외 처리부] 
END [함수 이름]; 
```

```sql
CREATE OR REPLACE FUNCTION - 이 친구는 함수를 생성시키는 역할을 한다.
매개변수 - 데이터를 받아주는 친구 & 항상 쓸 때, 매개변수 + 데이터 타입
return 데이터 타입 - function이 반환할 데이터의 타입을 지정한다.
BEGIN, END - function이 실행되는 구간 & 끝나는 구간
BEGIN ~ (RETURN) ~ END - RETURN 반환 값은 매개변수를 받고 연산 후, 최종적으로 반환될 값이다.
```

우리 초보 개발자들에게 기본형을 보고 바로 적용하라고 하는 건 머리를 혹사시킨다.

 

그렇기에 예제를 봐야지!!

진짜 간단하고 이해하기 쉽게 작성해보았다.

```sql
CREATE OR REPLACE FUNCTION test 
       (date DATE) RETURN VARCHAR2 IS 
       change_date VARCHAR2 (20); 
BEGIN 
    change_date := NULL;          
    change_date := TO_CHAR(date, 'YYYY-MM-DD'); 
    RETURN change_date; 
END;
```

test함수를 만든다. 

데이터를 받을 DATE형 매개변수 date를 만든다.

return 데이터형을 varchar2로 받고 return 할 매개변수 change_date를 만든다. (데이터형은 varchar2(20))

이제 연산을 시작한다. BEGIN

return할 매개변수 change_date에 TO_CHAR( 데이터 받을 매개변수명, '데이터 형식 변환')

change_date 리턴 끝.            

date로 넣은 값을 우리는 'YYYY-MM-DD' 형태로 만드는 함수를 만들어보았다.

 
그럼 이제 써봐야지

```sql
SELECT TO_CHAR(LIMIT_DATE, 'YYYY-MM-DD') AS deadline from example
```

- 기존의 이 sql문이 아래와 같이 사용될 수 있다.

```sql
SELECT test(SYSDATE) FROM dual;
```

test함수 안에 SYSDATE를 넣으면 아까 우리가 만들었던 함수가 실행되고 'YYYY-MM-DD'의 형태로 출력이 된다.

다른 sql문에서도 사용할 수 있으니 우리는 노가다성 코딩을 안 해도 된다!!!

출처: https://docu94.tistory.com/61

