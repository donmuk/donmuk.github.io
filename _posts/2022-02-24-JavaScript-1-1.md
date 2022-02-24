---
layout: post
title: "자바스크립트 Encoding 함수"

comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

[JavaScript] 문자 인코딩 함수 : escape() encodeURI() encodeURIComponent()  

 

escape(string) : ASCII(아스키) 문자를  유니코드 형식으로 변환

바이트는 %XX 2바이트는 %uXXXX형태로 변환

unescape(string) : 유니코드문자를 디코딩

encodeURI(string) : 주어진 문자열을 URI로 특수문자( :  ; / = ? & 등의 특수문자) 제외 encoding 한다.

decodeURI(string) : 주어진 문자열에서 encoding 된 URI를 decoding 한다.

encodeURIComponent(string) : 주어진 문자열을 URI로 모든 문자( :  ; / = ? & 등의 특수문자)를 encoding 한다.

decodeURIComponent(string) : 주어진 문자열에서 encoding 된 URI를 decoding 한다.



```
​Server-Side Javascript

<script language="javascript" runat="server">
function decodeUTF8(str){
 // 특수문자도 포함할 경우  decodeURIComponent(str) 를 사용. 

   return decodeURI(str); 
}
function encodeUTF8(str){
// 특수문자도 포함할 경우  encodeURIComponent(str) 를 사용.
     return encodeURI(str);
}
</script> 
```

- 실제 사용 CASE HISTORY

팝업을 obj를 넘겨 호출하는 경우에서 한글 데이의 경우 인코딩이 깨지는 현상 발생

- 해결방법
넘기는 파라미터를 인코딩하여 넘기고

팝업창에서 받을 때 다시 디코딩하여 사용하였음..


- encodeURI(string)

```
var obj = {
    "testData", "한글데이터"
    "testData2", "한글데이터2"
}

넘길때....

ex) var testData = encodeURI(obj.testData);

받을 때....

var testData2 = decodeURI(obj.testData)
```


 출처 : [https://rosalife.tistory.com/48]