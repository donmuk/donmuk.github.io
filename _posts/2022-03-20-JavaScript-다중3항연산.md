---
layout: post
title: "JavaScript 다중 3항 연산자 사용 방법"
comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

# 삼항 연산자(ternary operator)

## 삼항 연산자는 유일하게 피연산자를 세 개나 가지는 조건 연산자입니다.

 
### 문법
```
표현식 ? 반환값1 : 반환값2
```
 
물음표(?) 앞의 표현식에 따라 결괏값이 참이면 반환값1을 반환하고, 결괏값이 거짓이면 반환값2를 반환합니다.

- 예제

```
var x = 3, y = 5;
var result = (x > y) ? x : y   // x가 더 크면 x를, 그렇지 않으면 y를 반환함.
document.write("둘 중에 더 큰 수는 " + result + "입니다.");
```


## 다중 삼항 연산자 
```
(조건문1) ? (조건문 1이 참일때) : (거짓일때 수행할 조건문2) ? (조건문 2 참일때) : (모두 거짓일 때)

TEST01 == "A" ? "X" : TEST02 == "B" ? "XX" : TEST03 == "C" ? "XXX" : "ZZZ"
```




