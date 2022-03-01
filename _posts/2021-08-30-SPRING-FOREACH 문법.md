---
layout: post
title: "[MYBATIS] FOREACH 문법"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---

- FOREACH 문법

foreach

collection : 넘어온 파라미터의 반복하기 원하는 파라미터를 입력하여 주면 된다. 예를 들어 vo의 testMap이라는 Map이 있다면 collection에 testMap을 넣어주면 된다.

item : List의 경우 순차적으로 반복하여 값이 저장된다. item을 data라고 하였을 경우 WHERE col = #{data} 이런식으로 사용이 가능하다. Map에서는 key의 value가 저장된다.

separator : 반복 되는 사이에 출력 할 문자열

open : 해당 구문이 시작될때 삽입되는 문자열

close : 해당 구문이 종료될때 삽입되는 문자열

index : List의 경우 index 번호, Map의 경우 key 값이 저장된다.


- Mybatis에서 Map 조회

```sql
<select id="아이디 입력" parameterType="맵 or 리스트 or VO" resultType="원하는 타입 resultMap을 이용하여도 됨">
	SELECT TEST
    FROM TEST_TBL
    WHERE 
    <foreach collection="data" item="value" index="key" separator="and">
    	(KEY = #{key} AND VALUE = #{value})
    </foreach>
</select>
```

- Mybatis Map안에 List 조회

```sql
<select id="아이디 입력" parameterType="맵 or 리스트 or VO" resultType="원하는 타입 resultMap을 이용하여도 됨">
	SELECT TEST
    FROM TEST_TBL
    WHERE 
    <foreach collection="mapData" item="value" index="key" separator="AND">
    	#{key} IN
        <foreach collection="value" item="item" index="idx" separator="or"  open="(" close=")">
        	#{item}
        </foreach>
    </foreach>
</select>
```

- Mybatis에서 List 조회

```sql
<select id="아이디 입력" parameterType="맵 or 리스트 or VO" resultType="원하는 타입 resultMap을 이용하여도 됨">
	SELECT TEST
    FROM TEST_TBL
    WHERE 
    	TEST_VAL IN
    <foreach collection="list" item="value" index="idx" separator="," open="(" close=")">
    	#{value}
    </foreach>
</select>
```

```
public List<Books> getBooksInfo() {
            
    // List 선언
    List<String> codeList = new ArrayList<String>();
 
    codeList.add("01"); //List 정보 세팅
    codeList.add("05"); //List 정보 세팅
    
    // Map 선언       
    Map<String, Object> param = new HashMap<String, Object>();
 
    param.put("id", "1");        //#{id}에 셋팅
    param.put("name", "victor"); //#{name}에 셋팅

    //map에 list를 넣는다.
    param.put("code_list", codeList); 
    
    return sqlSession.selectList("selectBooksInfo", param);
    
}
```


```sql
<select id="selectBooksInfo" parameterType="java.util.HashMap" resultType="kr.co.husk.Books" >
    select
        price,
        discount_price
    from
        books
    where
        id = #{id} 
        and name = #{name}
        <choose>
            <when test="code_list.size != 0">
                and book_code in 
                <foreach collection="code_list" item="item" index="index" separator="," open="(" close=")">
                    #{item}
                </foreach>
            </when>
        </choose>
</select>

- 같은 결과 쿼리

select price
    	,discount_price
  from books
 where id = '1' 
	and name = 'victor'
	and book_code in ('01','05')


   출처: https://huskdoll.tistory.com/507
         https://ming9mon.tistory.com/152

