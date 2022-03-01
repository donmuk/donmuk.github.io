---
layout: post
title: "[ORACLE] MERGE INTO 사용법, 노하우 정리"
comments: true
description: ""
keywords: ""
tags: [DB]
---


오라클에서 쿼리문을 작성하다 보면, 하나의 쿼리문으로 INSERT, UPDATE, DELETE 작업을 해야 하는 경우가 있다. 

이럴 때에는 MERGE 문을 사용하면 간단하게 쿼리문을 작성할 수 있다. 

오라클 9i부터 MERGE 문을 사용할 수 있으며, DELETE 절은 10g부터 사용할 수 있다.
 
![675y7u43645341432155](/images/oracle/675y7u43645341432155.png)

```
오라클 MERGE 문
단일 테이블 사용법 (DUAL)
MERGE 
 INTO emp a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.ename, a.deptno)
      VALUES (7788, 'SCOTT', 20);
 ```

단일(자신의) 테이블에 MERGE 문은 자주 사용하므로 꼭 기억해 두는 것이 좋다. USING 절에 테이블 대신 dual을 사용하면 된다. 

ON 조건절이 일치하면 UPDATE, 불일치하면 INSERT를 하는 쿼리이다.

 
```
조인을 사용하는 방법
MERGE 
 INTO job_history a
USING emp b
   ON (a.empno = 7788 AND a.empno = b.empno)
 WHEN MATCHED THEN
      UPDATE
         SET a.job = b.job
           , a.deptno = b.deptno
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.job, a.deptno)
      VALUES (b.empno, b.job, b.deptno);
``` 

조인을 사용하는 방법은 UPDATE 문 대신 사용하는 경우가 많다. 

기본 UPDATE 문도 조인을 할 수 있지만 쿼리문이 조금 복잡해 지는 경향이 있다. 

MERGE 문을 사용하면 UPDATE 문 조인을 쉽게 사용할 수 있다.

 
```
인라인뷰 (서브쿼리)를 사용하는 방법
MERGE 
 INTO emp a
USING (SELECT aa.empno
            , aa.job
            , aa.deptno
         FROM emp aa
            , dept bb
        WHERE aa.empno = 7788
          AND aa.deptno = bb.deptno) b
   ON (a.empno = b.empno)
 WHEN MATCHED THEN
      UPDATE
         SET a.job = b.job
           , a.deptno = b.deptno
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.job, a.deptno)
      VALUES (b.empno, b.job, b.deptno);
``` 

서브쿼리의 결과와 조인하여 MERGE 문을 사용할 수 있다.

 
```
WHERE 절 사용
MERGE 
 INTO emp a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
       WHERE a.job = 'ANALYST';
``` 

오라클 10g 부터 UPDATE, DELETE 문에서 WHERE 절을 사용할 수 있다. INSERT 절에서 WHERE 절을 사용하면 오류가 발생한다.

 
```
DELETE 절 사용
MERGE 
 INTO emp a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
       WHERE a.job = 'ANALYST'
      DELETE
       WHERE a.job <> 'ANALYST';
``` 

오라클 10g부터 DELETE 문을 사용할 수 있다. WHERE 절을 사용하지 않고 DELETE 문만 작성하면 MATCHED 된 모든 데이터는 삭제된다.

 

- 주의사항

![66412341261241341241234](/images/oracle/66412341261241341241234.png)
 

ON 조건절에 사용할 컬럼을 업데이트하면 오류가 발생한다.

 
```
SQL 오류: ORA-38104: ON 절에서 참조되는 열은 업데이트할 수 없음: "A"."JOB"
38104. 00000 -  "Columns referenced in the ON Clause cannot be updated: %s"
```

출처 : hhttps://gent.tistory.com/406