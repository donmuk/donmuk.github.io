---
layout: post
title: "Ajax와 Axios 그리고 fetch"
comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---


우선 Ajax는 Asynchronous JavaScript And XML의 약자입니다. 

Ajax의 약자를 토대로 본래 의미를 살펴보면 JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 데이터를 주고받는 기술이라고 할 수 있습니다.
HTML 과 여타 기술들을 사용하는 새로운 접근법이라고 설명하기도 하죠.

Ajax를 통해 서버와 비동기적으로 통신함으로 인해 우리는 전체 웹페이지를 다시 불러오는 동기 방식과는 다르게 점진적으로 해당 부분의 사용자 인터페이스를 갱신할 수 있습니다.

간단하게 말해서 Ajax는 JavaScript에서 비동기 HTTP 통신이 가능하도록 해줍니다. 비동기 Http 통신이란 response와 request를 비동기 식으로 다룰 수 있다는 것을 의미합니다.

예를 들어 여러분이 페이스북에서 좋아요 버튼을 누를 때마다 페이지가 갱신이 된다면 음... 많이 불편하겠죠?

또는 구글에서 검색을 하는데 추천 검색어가 로드될때마다 페이지가 새로고침된다면 얼마나 짜증이 날까요?

우리는 비동기식으로 데이터를 주고받으며 위의 문제들을 해결하는 것입니다.

jQuery와 묶여서 불리는 경우가 많은데 어떻게 연관이 있는 것이죠?

좋은 궁금증입니다. 우리가 보통의 상황에서 Ajax라는 말을 사용할 때를 보면 JQuery의 Ajax를 일컫을 때가 많습니다.
그렇지만 이는 아주 조금 잘못되었습니다. Ajax라는 친구를 JQuery를 통해 보다 더 쉽게 사용할 수 있기에 우리는 JQuery와 Ajax를 함께 묶어서 말할 때가 많은 것 뿐이죠.
저의 포스팅 중 SSR vs CSR 당신의 선택은? (feat. 웹의 변천사)편을 보신다면 Ajax라는 기술 자체는 이미 그 이전 2005년부터 쓰여왔다는 것을 아실 수 있습니다. 심지어 정립된 시기는 기보다 앞선 1999년이었죠. 그렇지만 코드가 정말 매우매우매우매우 지저분했기에 이에 대한 보완이 필요했고, 그러던 도중 jQuery에서 Ajax를 편리하게 사용할 수 있도록 정립하면서 jQuery가 급부상하게 된것이죠. (물론 jQuery 코드 자체가 워낙 간단한 이유도 있습니다.) 또한 jQuery를 사용하여 Ajax를 구현할 경우 브라우저에 구애받지 않고 동일한 코드로 같은 작업을 구현할 수 있습니다.

- 순수 Ajax를 사용하는 경우

```
// use Ajax without Jquery

function reqListener (e) {
    console.log(e.currentTarget.response);
}

var oReq = new XMLHttpRequest();
var serverAddress = "https://jsonplaceholder.typicode.com/posts";

oReq.addEventListener("load", reqListener);
oReq.open("GET", serverAddress);
oReq.send();
```

- Jquery를 통해 Ajax를 사용하는 경우

```
// use Ajax with Jquery

var serverAddress = 'https://jsonplaceholder.typicode.com/posts';

// jQuery의 .get 메소드 사용
$.ajax({
    url: ,
    type: 'GET',
    success: function onData (data) {
        console.log(data);
    },
    error: function onError (error) {
        console.error(error);
    }
});
```
Jquery를 통해 Ajax를 사용할 때 코드가 훨씬 간단해지고 직관적이죠? 뿐만 아니라 그냥 Ajax만을 사용할 때는 브라우저 간 호환성에 대해 염두해두고 각기 다른 코드를 작성해야하는 경우가 있는데 Jquery를 사용할 경우 호환성에 대해서도 보장이 된답니다.

이제 Ajax에 대해 이해가 되셨나요? 다음으로는 Axios를 살펴보도록 하겠습니다.

- Axios

기존에 WEB에서 어떤 리소스를 비동기로 요청하기 위해서는 XHR(XML HTTP Request)객체를 사용했어야 했었는데, XHR은 잘 디자인되어 있는 API가 아닙니다. 요청의 상태나 변경을 구독하려면 Event를 등록해서 변경사항을 받아야 했고 요청의 성공, 실패 여부나 상태에 따라 처리하는 로직이 들어가기 좋지 않았습니다.
이를 보완하기 위해 HTTP 요청에 최적화 되어 있고 상태도 잘 추상화 되어 있는 api들이 생겨나기 시작했습니다. 대표적으로 Axios와 fetch가 그 예인데 중 Axios를 먼저 살펴보도록 하겠습니다.

Axios는 Promise based HTTP client for the browser and node.js
즉, node.js와 브라우저를 위한 HTTP통신 라이브러리입니다.
비동기로 HTTP 통신을 가능하게 해주며 return을 promise 객체로 해주기 때문에 response 데이터를 다루기도 쉽습니다.
react를 사용하시는 분들은 후술할 fetch보다 Axios를 많이 선호하는 편이죠.
그 이유에 대해서는 포스팅 마지막에 설명토록 하겠습니다.

```
Axios의 post method 구현

axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Yongseong',
    lastName: 'Kim'
  }
});
```

코드가 상당히 간단하죠??
이제 fetch api에 대해 살펴보겠습니다.

- fetch

fetch는 ES6부터 JavaScript의 내장 라이브러리로 들어왔습니다. promise기반으로 만들어졌기에 Axios와 마찬가지로 데이터를 다루는데 어렵지 않으며, 내장 라이브러리라는 장점으로 인해 상당히 편리하죠. 코드 또한 간단합니다.

```
fetch의 post method 구현

//fetch
const url ='http://localhost3000/test`
const option ={
   method:'POST',
   header:{
     'Accept':'application/json',
     'Content-Type':'application/json';charset=UTP-8'
  },
  body:JSON.stringify({
  	name:'sewon',
    	age:20
  })

  fetch(url,options)
  	.then(response => console.log(response))
```

- Axios vs fetch

필자는 그동안 프로젝트를 진행할 때 fetch를 사용하였습니다. 그러다가 지난 겨울 방학, 회사에서 인턴생활을 할때 apisauce라는 dependency를 사용하였는데 이를 뜯어보니 Axios 기반의 모듈이더군요. 그래서 Axios에 대해서 공부를 해보았습니다. 사실 fetch와 큰 차이점은 없으나 react를 개발하시는 많은 분들이 fetch보다는 Axios를 선호하죠. 그 이유에 대해서 제가 생각해보았고 Axios와 fetch 각기 장단점을 정리해보았습니다.

- Axios

장점 : 

1. response timeout 처리 방법이 있다. 

2. (fetch에는 존재하지 않는 기능) promise 기반으로 다루기가 쉽다

3. 크로스 브라우징에 신경을 많이썼기에 브라우저 호환성이 뛰어나다.


단점 :

1. 모듈 설치를 해줘야한다.

- fetch

장점

1. 내장 라이브러리이기에 별도의 import를 해줄 필요가 없다.

2. promise 기반으로 다루기가 쉽다.

3. 내장 라이브러리이기에 사용하는 프레임워크가 안정적이지 않을 때 사용하기 좋다.

단점

1. internet explorer의 경우에는 fetch를 지원하지 않는 버전도 존재한다. (브라우저 호환성이 상대적으로 떨어진다.)

2. 기능이 부족하다.

- 결론
일단 웹 프론트 프레임워크(react js,vue js 등)을 다룰때에는 Axios를 사용하는 것이 더 좋다고 생각합니다. Axios는 크로스 브라우징에 신경을 많이 쓴 모듈인게 눈에 보이며, 기능또한 우수하기 때문이죠. 전체적으로 제가 봤을 때 fetch의 상위 호환이라고 생각을 합니다. ✋ 다만, react-native에서는 fetch를 사용하는 것이 아주 조금 더 좋다고 생각합니다. react-native의 경우 아직 안정화 된 프레임워크가 아니기에 지속적인 version update가 진행되고 있고, axios에서 이를 반영하지 못할 경우 생각지 못한 에러가 발생할 수 있다는 우려 때문입니다.

이론적인 결론은 이렇지만 Ajax 툴은 가급적 모두 다 사용해보시는 것을 추천드립니다. 러닝 커브가 전혀 존재하지 않을 정도로 fetch와 Axios의 코드 차이가 미미하기도 하고 사실 크게 부담느낄정도로 장단점이 극명하지도 않기 때문이죠.

저는 현재 회사에서 사용하던 apisauce dependency를 이용해서 프로젝트를 진행중입니다.

```
//API.js
import {create} from 'apisauce';
import {yongseongURL} from '@env';

export const baseURL = yongseongURL;

const yongseongAPI = create({
  baseURL,
});

export default yongseongAPI;
//Auth.js

import yongseongAPI from './API';

const getMyInfo = async (token) => {
  try {
    return await yongseongAPI.get('/users/me/', null, {
      headers: {
        'Content-Type': 'application/json',
        Authentication: token,
      },
    });
  } catch (error) {
    console.log(error);
  }
};
```

출처: https://velog.io/@kysung95/%EA%B0%9C%EB%B0%9C%EC%83%81%EC%8B%9D-Ajax%EC%99%80-Axios-%EA%B7%B8%EB%A6%AC%EA%B3%A0-fetch
