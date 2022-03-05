---
layout: post
title: "[MSSQL] WITH (NOLOCK)"
comments: true
description: ""
keywords: ""
tags: [DB]
---

MSSQL에서 SELECT 시에 WITH (NOLOCK) 을 주면 공유잠금을 걸지 않고 바로 조회를 한다.

MSSQL은 기본적으로 SELECT 시에 공유잠금이 걸린다. 즉, SELECT 문이 수행되는 테이블에 대해서 INSERT, UPDATE, DELETE 문이 수행되고 있다면 SELECT문은 선행 작업이 모두 끝날때까지 LOCK이 걸린다. 이때 SELECT 문에 WITH (NOLOCK)을 추가하면 선행작업의 결과와 관계없이 바로 SELECT문이 수행되어서 결과를 반환하게 된다.

```
SELECT * FROM TABLE1 WITH (NOLOCK)
```
 

SELECT 문장에서 여러 테이블을 조인해서 가져오는 경우 WITH (NOLOCK)을 사용하기 위해서는 모든 테이블에 적어주어야 한다.

프로시저 내에서 사용되는 SELECT 문에서 WITH (NOLOCK)을 사용하기 위해서는 각 문장마다 삽입할 필요없이 프로시저 시작 부분에 다음 문장을 추가해 주면 된다.

```
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```
 

- 예제)

```
CREATE PROCEDURE 프로시저명

AS
    SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
    SET NOCOUNT ON;
BEGIN

    ...


END
```

출처: https://roadrunner.tistory.com/238 

