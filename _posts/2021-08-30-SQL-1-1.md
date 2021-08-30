---
layout: post
title: "[ORACLE] PIVOT, UNPIVOT"
comments: true
description: ""
keywords: ""
tags: [DB]
---

- PIVOT

```sql
SELECT * 
  FROM (피벗 할 쿼리문)
 PIVOT(그룹함수(칼럼) 
   FOR 피벗 할 칼럼
    IN (항목1, 항목2, 항목3 ...)
 );
 
  SELECT * 
    FROM (SELECT job
               , deptno 
               , sal   
            FROM emp ) --피벗 할 쿼리문
   PIVOT( SUM(sal)   --그룹함수
          FOR deptno --기준칼럼
           IN ('10', '20', '30') --기준칼럼항목
        )
```

![PIVOT](/images/oracle/PIVOT.png)


- UNPIVOT

```sql
WITH temp AS (
    SELECT 1 AS col1, 2 AS col2, 3 AS col3 FROM dual
)

 SELECT col_nm
              , col_val
     FROM (
                   SELECT *
                      FROM temp
                 )
UNPIVOT (col_val FOR col_nm IN (col1, col2, col3))
```

![PIVOT](/images/oracle/UNPIVOT.png)
