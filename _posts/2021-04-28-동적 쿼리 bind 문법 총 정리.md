---
layout: post
title: "[MyBatis] 동적 쿼리 <bind> 문법 총 정리"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---


<bind>
 

개념

외부에서 전달된 파라미터를 이용하여 변수 생성하는 엘리먼트

동적 쿼리 변수를 생성할 때 사용한다.


조건

MyBatis version 3.2.3 이상


문법

동적 쿼리문 안에 작성

<select | insert | update | delete>

<bind name="지정할 변수이름" value="파라미터 값+부가 옵션"/>

</select | insert | update | delete>

name 속성 : 자기가 지정할 변수 이름

value 속성 : 받아오는 파라미터 값 + 추가 문법 (이때 문법은 OGNL(Object Graph Navigation Language) 표현식을 사용한다.)

 

 

실전 예시 1. Like 문 문법

Parameter 01 : id

Parameter 02 : subject

```sql
<select id="getTest" resultType="board">
    SELECT * 
      FROM board
      <bind name="ids" value="'%'+id+'%'"/>
      <bind name="subjects" value="'%'+subject+'%'"/>
      <where>
          <if test="id != null"> 
              AND id like #{ids}
          </if>
          <if test="subject != null"> 
              AND subject like #{subjects} 
          </if>
    </where>
</select>
``` 

이런식으로 value 속성에 받아온 파라미터를 작성하고 추가 문법을 덧붙이면 됩니다.

 

실제 실행된 쿼리

SELECT * FROM board WHERE id like '%2%' AND subject like '%test%'

실전 예시 2. Map 사용 문법

<bind name="a" value="hMap.get('a'.toString())"/>

<bind name="b" value="hMap.get('b'.toString())"/>

<bind name="c" value="hMap.get('c'.toString())"/>

실전 예시 3. 메서드(Method) 사용 문법

 

- bind 사용 전

```sql
<select id="selectPerson" parameterType="String" resultType="hashmap">
  SELECT * 
    FROM PERSON 
  WHERE FIRST_NAME like #{name.upperCase() + '%'} 
</select>
```

- bind 사용 후

```sql
<select id="selectPerson" parameterType="String" resultType="hashmap">
  <bind name="nameStartsWith" value="_parameter.getName().upperCase() + '%'"/>
      SELECT * 
        FROM PERSON 
        WHERE FIRST_NAME like #{nameStartsWith} 
</select>
```

출처: https://java119.tistory.com/90?category=824525

