---
layout: post
title: "[MyBatis] 동적 쿼리 where 문법 총 정리"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---



개념

추가 쿼리 문의 내용을 where 문 안에 작성하면 첫 조건이 AND로 시작할지라도 WHERE로 치환해준다.


조건

MyBatis version 3.2.3 이상

 

문법

```
sql
<select id="id">

SELECT * FROM table

<where>
추가 SQL
</where>

</select>
```

잘못된 문법 예시 1.

```
sql
<select id="getTest" resultType="board">

SELECT * FROM board

<if test="id != null">WHERE id = #{id} </if>
<if test="subject != null">AND subject = #{subject} </if>

</select>
```

만약 이런 식으로 작성하면

 

1-1."subject" 칼럼이 먼저 올 경우

SELECT * FROM board AND subject = #{subject}
문법 에러가 나게 된다. 이때 <where> 문을 써주면 된다.

실전 예시 1.

```
sql
<select id="getTest" resultType="board">

SELECT * FROM board

<where>
<if test="id != null">AND id = #{id} </if>
<if test="subject != null">AND subject = #{subject} </if>
</where>

</select>
```

이런식으로 작성하게 되면

 

실제 쿼리는 이런 식으로 작성된다.

 
```
sql
1-1."id" 칼럼 값만 있을 경우

SELECT * FROM board WHERE id = #{id}
1-2."subject" 칼럼 값만 있을 경우

SELECT * FROM board WHERE subject = #{subject}
1-3. 두 컬럼 모두 값이 있을 경우

SELECT * FROM board WHERE id = #{id} AND subject = #{subject}

#WHERE AND가 되지 않고 알아서 문법에 맞게 치환 해줍니다.
```

 

그냥 (AND || OR ) 뭐가 먼저 들어올지 모르는 매개변수 값에 <where> 문을 쓰면 됩니다. 

출처: https://java119.tistory.com/90?category=824525

