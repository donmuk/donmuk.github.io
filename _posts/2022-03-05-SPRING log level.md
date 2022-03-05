---
layout: post
title: "[MSSQL] WITH (NOLOCK)"
comments: true
description: ""
keywords: ""
tags: [DB]
---

회사에서 업무를 하게 되면

무분별한 로그때문에 로그 확인시

어려움을 겪게된다.

(불필요한 로그로 인해 비즈니스 로그를 찾기 힘듦)

그래서 정리하게 된 Logging Level(로그레벨).

​

로깅레벨은 ALL,OFF포함 7단계지만

대개 아래 5단계로 말을한다.

​

DEBUG

INFO

WARN

ERROR

FATAL

​

ALL < DEBUG < INFO < WARN < ERROR < FATAL < OFF

​

WARN을 로그 레벨로 지정을 하게 되면 그 아래

WARN, ERROR, FATAL까지 로그가 찍히게 된다.

​
```
import org.apache.log4j.*;

public class LogClass {
   private static org.apache.log4j.Logger log = Logger.getLogger(LogClass.class);
   
   public static void main(String[] args) {
      log.setLevel(Level.WARN);// WARN 을 로그레벨로 지정

      log.trace("Trace Message!");
      log.debug("Debug Message!");
      log.info("Info Message!");
      log.warn("Warn Message!"); // WARN 보다 높은 레벨이 로그에 찍힌다.
      log.error("Error Message!");
      log.fatal("Fatal Message!");
   }
}
```

- 결과값

```
Warn Message!
Error Message!
Fatal Message!
```

로그레벨은 Config 파일을 통해 세팅 가능.

​

로그레벨의 자세한 내용은 아래 링크 참고!

``` 
log4j - Logging Levels - Tutorialspoint
log4j - Logging Levels Advertisements Previous Page Next Page The org.apache.log4j.Level levels. You can also define your custom levels by sub-classing the Level class. Level Description ALL All levels including custom levels. DEBUG Designates fine-grained informational events that are most useful t...
```

www.tutorialspoint.com



- 로깅(Logging) 

- 로그(Log)란 프로그램 개발이나 운영 시 발생하는 문제점을 추적하거나 운영 상태를 모니터링하기 위한 텍스트이다.

- 지금까지는 System.out.println(); 문을 사용하여 로그를 기록했으나 이보다 로그를 기록하는 클래스를 만들어 사용하는 것이 더 나은 방법이다.

- Log4j2 는 다음과 같은 로그 레벨을 가진다. 

- TRACE > DEBUG > INFO > WARN > ERROR > FATAL

- INFO로 셋팅하면, INFO, WARN, ERROR, FATAL은 기록된다.

FATAL : 아주 심각한 에러가 발생한 상태를 나타낸다.

ERROR : 어떠한 요청을 처리하는 중 문제가 발생한 상태를 나타낸다.

WARN : 프로그램의 실행에는 문제가 없지만, 향후 시스템 에러의 원인이 될 수 있는 경고성 메시지를 

나타낸다.

INFO : 어떠한 상태 변경과 같은 정보성 메시지를 나타낸다.

DEBUG : 개발시 디버그 용도로 사용하는 메시지를 나타낸다.

TRACE : 디버그 레벨이 너무 광범위한 것을 해결하기 위해서 좀 더 상세한 이벤트를 나타낸다. 

- log4j 에서 사용하는 log 레벨은 6단계 이다.

- TRACE 으로 설정하면 모든 레벨의 로그가 전부 기록되지만, FATAL 으로 설정하면 

FATAL 보다 하위 수준의 로그는 기록되지 않는다.

출처: https://blog.naver.com/PostView.nhn?blogId=2zino&logNo=221641662104
      https://yunyoung1819.tistory.com/30

