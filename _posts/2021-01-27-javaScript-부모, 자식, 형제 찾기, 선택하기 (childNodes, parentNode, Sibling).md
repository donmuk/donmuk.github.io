---
layout: post
title: "[Javascript] 부모, 자식, 형제 찾기, 선택하기 (childNodes, parentNode, Sibling)"
comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

## childNodes

```
<div id=mother>
    <div id=one></div>
    <div id=two></div>
    <div id=three></div>
</div>
```
 
```
// 부모노드
var mother = document.getElementById('mother');
// 모든 자식 노드
var every = mother.childNodes;
// 첫번째 자식 노드
var one = mother.firstChild;
// 세번째 아들내미 찾기
var three = mother.lastChild;
```
 

- 모든 자식은 chindNodes 를 통해 찾을 수 있다.

- getElementsByClassName 처럼 배열형태로 저장된다.

 
#### 첫번째 자식은 firstChild

#### 마지막 (늦둥이) 자식은 lastChild

로 알아낼 수 있는데, 이 구간에서 위아래에 끼어있는 두번째 자식의 슬픔이 여기까지 들려온다. 
필자도 두번째 자식이기 때문에, 오직 chindNodes 함수로만 찾을 수 있는 존재감없는 인생일 뿐이다.

## parentNode

- 부모노드

```
var one = document.getElementById('one');
var mother = one.parentNode;
```
 

- 부모 노드 찾기
 
```
var one = document.getElementById('one');
var grandparents = one.parentNode.parentNode;
```
 

## Sibling

``` 
var one = document.getElementById('one');
var two = one.nextSibling;
var one = two.previousSibling;
```
 
말그대로 nextSibling은 밑에 있는 형제를 previousSibling은 위에 있는 형제의 엘리먼트를 선택한다.

* 기능 실행시 ex) function onClickevent(this){ ... }; 이런식으로 넘겨줄 경우 해당 TAG 엘리먼트 요소를 전달한다.
  전달 받은 엘리먼트 요소를 활용하여 자바스크립트를 적용시킬 수 있다. 예를 들어 그리드 하나의 셀에 1개, 2개의 버튼을 보이기, 안보이기 처리 등등....


출처: https://itun.tistory.com/501

