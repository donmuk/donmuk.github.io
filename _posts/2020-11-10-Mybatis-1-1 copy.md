---
layout: post
title: "Mybatis 에서 #{} 과 ${}의 차이"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---

## #{ }

파라매터가 String 형태로 들어와 자동적으로 '파라메터' 형태가 된다. 

예를들어, #{user_id}의 user_id의 값이 abc라면 쿼리문에는 USER_ID = 'abc'의 형태가 된다.

쿼리 주입을 예방할 수 있어 보안측면에서 유리하다.

## ${ }

파라매터가 바로 출력된다.

해당 컬럼의 자료형에 맞추어 파라매터의 자료형이 변경된다.

쿼리 주입을 예방할 수 없어 보안측면에서 불리하다. 그러므로, 사용자의 입력을 전달할때는 사용하지 않는 편이 낫다.

테이블이나 컬럼명을 파라메터로 전달하고싶을 때 사용한다. 

#{}은 자동으로 ''가 붙어서 이 경우에는 사용할 수 없다.


출처 : https://logical-code.tistory.com/25