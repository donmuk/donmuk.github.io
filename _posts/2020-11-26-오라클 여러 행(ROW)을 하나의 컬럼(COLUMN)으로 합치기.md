---
layout: post
title: "오라클 여러 행(ROW)을 하나의 컬럼(COLUMN)으로 합치기"
comments: true
description: ""
keywords: ""
tags: [DB]
---

WM_CONCAT 함수를 이용하여 손쉽게 여러행의 데이터를 한 컬럼으로 합칠 수 있다.

□ 방법 1. (WM_CONCAT 이용)

- 가상 테이블

```sql
WITH TEST_TABLE AS (
    SELECT '고구려' COUNTRY, '1대' ST, '동명성왕'   KING_NM FROM DUAL UNION ALL
    SELECT '고구려' COUNTRY, '3대' ST, '대무신왕'   KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '1대' ST, '온조왕'     KING_NM FROM DUAL UNION ALL
    SELECT '고구려' COUNTRY, '2대' ST, '유리왕'     KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '3대' ST, '기루왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '1대' ST, '남해왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '1대' ST, '박혁거세'   KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '2대' ST, '다루왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '1대' ST, '유리이사금' KING_NM FROM DUAL
)
 
-- 조회 쿼리
SELECT COUNTRY
     , WM_CONCAT(KING_NM) KING_NM
  FROM TEST_TABLE
 GROUP BY COUNTRY
```

- WM_CONCAT  함수를 이용하여 여러행의 값을 하나의 컬럼으로 합칠 수 있다.

- 값의 구분은 쉼표(,)로 구분되어 합쳐진다.

□ 방법 2. (XMLAGG, XMLELEMENT 이용)

- 가상 테이블

```sql
WITH TEST_TABLE AS (
    SELECT '고구려' COUNTRY, '1대' ST, '동명성왕'   KING_NM FROM DUAL UNION ALL
    SELECT '고구려' COUNTRY, '3대' ST, '대무신왕'   KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '1대' ST, '온조왕'     KING_NM FROM DUAL UNION ALL
    SELECT '고구려' COUNTRY, '2대' ST, '유리왕'     KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '3대' ST, '기루왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '2대' ST, '남해왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '1대' ST, '박혁거세'   KING_NM FROM DUAL UNION ALL
    SELECT '백제'   COUNTRY, '2대' ST, '다루왕'     KING_NM FROM DUAL UNION ALL
    SELECT '신라'   COUNTRY, '3대' ST, '유리이사금' KING_NM FROM DUAL
)

--조회 쿼리
SELECT COUNTRY
     , SUBSTR(
        XMLAGG(
            XMLELEMENT(COL ,',', KING_NM) ORDER BY ST).EXTRACT('//text()'
        ).GETSTRINGVAL()
       , 2) KING_NM
  FROM TEST_TABLE
 GROUP BY COUNTRY
 ```

- 첫번째 방법에 비하여 조금 복잡해 보이나 합쳐지는 값의 구분자를 쉼표(,) 뿐만 아니라 사용자가 직접 값의 구분자를 지정 할 수 있다. 또한 해당 값의 정렬을 지정 가능 하다. 

 
□ 결과

○ 조회 전 (TEST_TABLE)

![16123124124](/images/oracle/16123124124.png)


○ 조회 후

![568463546346](/images/oracle/568463546346.png)


## 정리
    - XMLAGG 
	  ==> xmlagg는 xml 문서를 만들어주는 함수

	- XMLELEMENT
	  ==> 주어진 태그로 값을 감싸 하나의 xml 엘리먼트를 만듬
	  
    - .EXTRACT
	  ==> TAG삭제 
	  
    - .GETSTRINGVAL()
	  ==> String으로 형변환


출처 : https://gent.tistory.com/15