---
layout: post
title: "JavaScript 기본"
comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

## DATE

var now = new Date(); //현재 날짜와 시간(시, 분 초) 값으로 초기화된 객체 생성

var startdy = new Date(2017, 2, 1); //학기 시작일. 2017년 3월1일(2는 3월을 뜻함)

var now = new Data(); //현재 2017년 5월 15일 저녁 8시 48분이라면
 
var date = now.getData(); //오늘 날짜. date = 15

var hour = now.getHours(); //지금 시간. hour = 20

## String

var hello = new String("Hello");

var every = "Boy and Gril";

var m = hello.length; // m은 5

var n = every.length; // n은 12

var n = "Thank you".lengh; // n은 9

var hello = new String("Hello");

var c = hello[0]; // c = "H". 문자 H가 아니라 문자열 "H", 자바스크립트는 문자 타입 없음

### 서브스트링 리턴

var a = every.slice(5, 8); // 인덱스 5에서 8 이전의 문자열 "and" 리턴

var b = every.substr(5, 3); // 인덱스 5부터 3개의 문자로 구성된 "and" 리턴

var c = every.substring(5, 8); // 인덱스 5부터 8 이전의 문자열 "and" 리턴

### 문자열변경

var d = every.replace("and", "or");

var daughter = "황기태".replace("기태", "수희");

### 대소문자 변경

var upper = every.toUpperCase(); // upper = "BOYS AND GIRLS"

var lower = every.toLowerCase(); // lower = "boys and girs"

## 문자열 분할

var suv = every.split(" " );

// 실행 결과 3개의 문자열을 가진 배열 sub생성. sub[0]="BOYS", sub[1]="and", sub[2]="Girls"

// sub 배열의 원소 개수는 3, sub.length로 알 수 있음

### 문자열 압뒤 공백 제거

var name = " kitae ";

name = name.trim(); // trime()은 앞뒤 공백을 제거하여 "kitae" 리턴

