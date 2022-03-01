---
layout: post
title: "[MyBatis] 동적 쿼리 foreach문 문법 총 정리"
comments: true
description: ""
keywords: ""
tags: [SPRING]
---

*ibatis iterate문 지원 태그

 

property : 파라미터명

prepend : 쿼리로 쓰일 문자

open : 구문이 시작될때 삽입할 문자열

close : 구문이 종료될때 삽입할 문자열

conjunction : 반복되는 사이에 출력할 문자열


*ibatis : MyBatis의 옛 버전

MyBatis foreach문 지원 태그

 

collection : 전달받은 인자. List or Array 형태만 가능

item : 전달받은 인자 값을 alias 명으로 대체

open : 구문이 시작될때 삽입할 문자열

close : 구문이 종료될때 삽입할 문자열

separator : 반복 되는 사이에 출력할 문자열

index : 반복되는 구문 번호이다. 0부터 순차적으로 증가

 

즉, ibatis iterate -> MyBatis foreach로 변경됐습니다.

 

 

문법

<foreach collection="List or Array" item="alias" ></foreach>
 

사용 예시

<select id="selectPostIn" resultType="domain.blog.Post">
    SELECT *
      FROM POST P
     WHERE ID in
     <foreach item="item" index="index" collection="list"
         open = "(" separator="," close=")"> #{item}
     </foreach>
</select>

-참고 자료 MyBatis 공식 사이트

1. 배열 예시

공통 배열 데이터 

String[] userArray = {"1","2","3"}
 

1-1. 배열 파라미터를 Map을 통해 넘겼을 경우

DAO

```
public List<Members> getAuthUserList(String[] userArray) {
		HashMap<String, Object> map = new HashMap<String, Object>();
		map.put("userArray",userArray);
		return sqlSession.selectList("getAuthUserList", map);
}
```

user-Mapper.xml

```
<select id="getAuthUserList"  resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="userArray" item="arr" open="(" close=")" separator=",">
 #{arr}
</foreach>
ORDER BY m.authority;
</select>
```

※ 주의 : collection을 꼭! 넘겨주는 배열 변수 값과 동일하게 작성하셔야 합니다.

1-2. 배열 파라미터를 직접 넘겼을 경우

 

DAO

```
public List<Members> getAuthUserList(String[] userArray) {
		return sqlSession.selectList("getAuthUserList", userArray);
}
```

user-Mapper.xml

```
<select id="getAuthUserList"  resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="array" item="arr" open="(" close=")" separator=",">
 #{arr}
</foreach>
ORDER BY m.authority;
</select>
```

※ 주의 : collection을 꼭! "array"로 작성하셔야 합니다.

 

2. 리스트 예시

 
공통 리스트 데이터 
```
List<Members> chkList = userService.getUserList(); //SELECT * FROM members 결과값
``` 

2-1. 리스트 Map을 통해 넘겼을 경우

 

DAO

```
public List<Members> getListTest(List<Members> chkList) {
		HashMap<String, Object> map = new HashMap<String, Object>();
		map.put("chkList",chkList);
		return sqlSession.selectList("getListTest", map);
}
```

user-Mapper.xml

```
<select id="getListTest" resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="chkList" item="item" open="(" close=")" separator=",">
 #{item.authority}
</foreach>
ORDER BY m.authority;
</select>
```

리스트 안에 뽑고 싶은 결괏값을 {key.value} 형태로 뽑으시면 됩니다.

※ 주의 : collection을 꼭! 넘겨주는 리스트 변수 값과 동일하게 작성하셔야 합니다.

 

2-2. 리스트 파라미터를 직접 넘겼을 경우

 

DAO
```
public List<Members> getListTest(List<Members> chkList) {
		return sqlSession.selectList("getListTest", chkList);
}
```

user-Mapper.xml

```
<select id="getListTest" resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="list" item="item" open="(" close=")" separator=",">
 #{item.authority}
</foreach>
ORDER BY m.authority;
</select>
```

리스트 안에 뽑고 싶은 결괏값을 {key.value} 형태로 뽑으시면 됩니다.

※ 주의 : collection을 꼭! "list"로 작성하셔야 합니다.


공통 설명

1. 먼저 리스트/배열 변수 값을 collection에 넣어주고, item이라는 설정으로 별칭 설정을 해준다.

2. 리스트/배열의 값이 시작하기 전 open="(" 이 설정돼있으므로'(' (열린 괄호)가 열리게 되고

3. 리스트/배열의 값이 한 번씩 반복문을 거칠 때마다 separator 옵션에 있는 ', '(콤마)가 찍히게 된다.

4. 반복이 끝나면 close=")" 설정이 있으므로 ')' (닫힌 괄호)가 쓰인다.

더욱 자세한 이해를 위해 실전 예시

 

문법에 들어가기 앞서, 공통 테이블 예시

예시 테이블

![1612312312412512312.png](/images/oracle/1612312312412512312.png)


예시 테이블 02

![6161512312512312312.png](/images/oracle/6161512312512312312.png)


[배열(array)] 예시 1. 멤버 테이블에서 체크박스에 따라 권한별 보여주는 기능 만들기
닫기
JSP

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script> 
<meta charset="UTF-8">
<script type="text/javascript">
var chkArray = new Array;
$(document).ready(function(){

	getAuthUserList($("input[name=chk_authority]:checked").val());
	
$("input[name=chk_authority]").change(function(){

	var chk = "";
	chkArray = [];
	
	$("input[name=chk_authority]:checked").each(function() {
	
		chkArray.push($(this).val());

	});
	
		
	if(chkArray.length == 0){
		$("#userList").empty();
	} else {
		getAuthUserList(chkArray);
	}
	
	});

});

function getAuthUserList(chkArray){
	
$.ajax({
		
		url : "/user/getAuthUserList",
		data : { chkArray : chkArray },
		traditional : true,
		async: true,
		success : function(data){
			var html = '';
			
			for(key in data){
			html += '<tr>';
			html += '<td>'+data[key].userId+'</td>';
			html += '<td>'+data[key].nickname+'</td>';
			html += '<td>'+data[key].name+'</td>';
			html += '</tr>';
			}
			
			
			$("#userList").empty().append(html);
			
		}
	})
	
}

</script>
</head>
<body>

<div>
유저 등급별 보기<br>
<input type="checkbox" name="chk_authority" value="0" checked="checked">실버
<input type="checkbox" name="chk_authority" value="1">골드
<input type="checkbox" name="chk_authority" value="2">플래티넘
<input type="checkbox" name="chk_authority" value="3">다이아
<input type="checkbox" name="chk_authority" value="4">마스터
<input type="checkbox" name="chk_authority" value="5">그랜드마스터

<table>
<thead>
<tr>
<th>아이디</th>
<th>닉네임</th>
<th>권한</th>
</tr>
</thead>
<tbody id="userList">
</tbody>
</table>
</div>
</body>
</html>
```

```
Controller

@RequestMapping("user/getAuthUserList")
	public @ResponseBody List<Members> getAuthUserList(String[] chkArray) {
		List<Members> result = userService.getAuthUserList(chkArray);
		return result;
}
Service

List<Members> getAuthUserList(String[] chkArray);
Servicelmpl

@Override
public List<Members> getAuthUserList(String[] chkArray) {
		return userDAO.getAuthUserList(chkArray);
}
DAO

public List<Members> getAuthUserList(String[] chkArray) {
		HashMap<String, Object> map = new HashMap<String, Object>();
		map.put("chkArray",chkArray);
		return sqlSession.selectList("getAuthUserList", map);
}
```

user-Mapper.xml
```
<select id="getAuthUserList"  resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="chkArray" item="item" open="(" close=")" separator=",">
 #{item}
</foreach>
ORDER BY m.authority;
</select>
```

쿼리 결과
```
SELECT m.*,a.name FROM members AS m  
JOIN authority AS a ON m.authority = a.authority 
WHERE m.authority IN
```

(체크박스 수에 따른 값)
#('1','2','3')
#('2','3')
ORDER BY m.authority;
쿼리 결과 설명

1. 먼저 배열 변수 값을 collection에 넣어주고, item이라는 설정으로 별칭 설정을 해준다.

2. 배열의 값이 시작하기 전 open="(" 이 설정돼있으므로'(' (열린 괄호)가 열리게 되고

3. 배열의 값이 한 번씩 반복문을 거칠 때마다 separator 옵션에 있는 ', '(콤마)가 찍히게 된다.

4. 반복이 끝나면 close=")" 설정이 있으므로 ')' (닫힌 괄호)가 쓰인다.

 

View

![154113671.png](/images/oracle/154113671.png)
![154113672.png](/images/oracle/154113672.png)
![154113673.png](/images/oracle/154113673.png)


[리스트(List)] 예시 2. 멤버 테이블에서 데이터 넘겨받아 리스트로 반복문 돌리기
닫기

Controller
```
@RequestMapping("user/getUserList")
	public @ResponseBody List<Members> getUserList() {
		List<Members> result = userService.getUserOne();
		List<Members> foreachTest = userService.getListTest(result);
		System.out.println(foreachTest);
		return result;
}
Service

List<Members> getListTest(List<Members> result);
Servicelmpl

@Override
public List<Members> getListTest(List<Members> result) {
		return userDAO.getListTest(result);
}
DAO

public List<Members> getListTest(List<Members> chkList) {
		HashMap<String, Object> map = new HashMap<String, Object>();
		map.put("chkList",chkList);
		return sqlSession.selectList("getListTest", map);
}
```
user-Mapper.xml

```
<select id="getListTest" resultType="members">
SELECT m.*,a.name FROM members AS m 
JOIN authority AS a ON m.authority = a.authority
WHERE m.authority IN
<foreach collection="chkList" item="item" open="(" close=")" separator=",">
 #{item.authority}
</foreach>
ORDER BY m.authority;
</select>
```

쿼리 결과

```
SELECT m.*,a.name FROM members AS m  
JOIN authority AS a ON m.authority = a.authority 
WHERE m.authority IN
```

(체크박스 수에 따른 값)

#('1','2','3')

#('2','3')

ORDER BY m.authority;

쿼리 결과 설명

1. 먼저 리스트 변수 값을 collection에 넣어주고, item이라는 설정으로 별칭 설정을 해준다.

2. 리스트의 값이 시작하기 전 open="(" 이 설정돼있으므로'(' (열린 괄호)가 열리게 되고

3. 리스트의 값이 한 번씩 반복문을 거칠 때마다 separator 옵션에 있는 ', '(콤마)가 찍히게 된다.

4. 반복이 끝나면 close=")" 설정이 있으므로 ')' (닫힌 괄호)가 쓰인다.



출처: https://java119.tistory.com/85?category=824525

