---
layout: post
title: "자바스크립트 배열, Object 리턴"

comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---
 

- 배열로 리턴하는 방법

```
function newCodes() {
    var dCodes = fg.codecsCodes.rs;
    var dCodes2 = fg.codecsCodes2.rs;
    return [dCodes, dCodes2]; 
};
```

- 배열리턴 접근방법

var codes = newCodes();
var dCodes = codes[0];
var dCodes2 = codes[1];

- Object 리턴하는 방법

```
var newCodes = function() {
    var dCodes = fg.codecsCodes.rs;
    var dCodes2 = fg.codecsCodes2.rs;
    return {
        dCodes: dCodes,
        dCodes2: dCodes2
    };
};
```

- Obect 리턴 접근방법

```
var codes = newCodes();
var dCodes = codes.dCodes;
var dCodes2 = codes.dCodes2;
```


 출처 : [https://hashcode.co.kr/questions/1397/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C-%EC%97%AC%EB%9F%AC%EA%B0%92%EC%9D%84-%EB%A6%AC%ED%84%B4%ED%95%B4%EC%95%BC%ED%95%98%EB%8A%94%EB%8D%B0-%EC%96%B4%EB%96%BB%EA%B2%8C%ED%95%B4%EC%95%BC%ED%95%98%EC%A3%A0]