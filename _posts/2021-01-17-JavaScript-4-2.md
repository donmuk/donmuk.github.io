---
layout: post
title: "자바스크립트 forEach 함수"

comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

forEach의 구문은 아래와 같습니다.
 
```
array.forEach(callbackFunction(currnetValue, index, array), thisArg);
```
 
매개변수로는 callbackFunction 과 thisArg 가 있습니다.

array의 인자값 만큼 callbackFunction이 반복되고,  thisArg 는 거기서의 this 값이 되는것이죠.

callbackFunction에는 3개의 매개변수가 있습니다.

currentValue : 배열 내 현재 값.

index : 배열 내 현재 값의 인덱스.

array : 현재 배열 입니다.

 

그럼 예를 보도록 하죠.

```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5];
    testArray.forEach(function(currentValue, index, array){
    console.log(currentValue);
    console.log(index);
    console.log(array);
    });
})(); 
</script>
```
 

아래는 위 test함수의 실행 결과 입니다.

![456789615](/images/javascript/456789615.jpg) 

이해가 되시나요?

배열의 첫번째 인자부터 반복되며 값이 들어오고 있습니다.

그럼 이를 활용해 배열 내 값의 합을 구해보도록 하겠습니다.

```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5];
    var sum = 0;
    function getSum(value){
        sum += value;
    }
    testArray.forEach(getSum);
    console.log(sum);
})();
</script> 
```

getSum 함수를 callbackFunction 으로 사용했습니다. 그러니 getSum 함수의 value는 currentValue 가 되겠죠.

배열 내 인자 수 만큼 반복되어 sum 변수에 더해지게 됩니다.

그럼 15라는 값이 출력되겠죠.

 

같은 함수를 thisArg를 활용해 작성 해 보도록 하겠습니다.

 
```
<script type="text/javascript">
(function test(){
    function Counter(){
        this.sum = 0;
    }
    Counter.prototype.add = function(array){
        array.forEach(function(currentValue){
            this.sum += currentValue;
        }, this);
    }
    var obj = new Counter();
    console.log(obj.sum);
    obj.add([1,2,3,4,5]);
    console.log(obj.sum);
})();
</script> 
```
 

위의 예를 이해하기 위해선 프로토타입에 대한 이해가 있어야 할 것 같은데요.

아래의 Link를 참고하시기 바랍니다.

 

Link : javascript prototype 프로토타입 이란? prototype을 사용하는 방법을 알아보자.

 

Counter에 프로토타입에 add라는 함수를 추가했고 그 함수가 forEach를 사용해 sum 값을 구하게 되죠.

그리고 thisArg에 this를 보냈습니다. 여기서의 this는 Counter function이 됩니다. 그러니 Counter의 this.sum에 값을 더하게 되는 것이죠.

 

첫번째 console.log(obj.sum)은 0값이 출력됩니다. add 함수가 실행되기 전이기 때문이죠.

(obj.add 함수를 호출할땐 프로토타입체이닝이 발생합니다.)

하지만 add 함수를 실행한 후에는 15값이 출력됩니다.

 

사용하는 언어의 for문과 비슷하다고 생각하시면 됩니다. (jquery의 each와 같은 기능)

 

이처럼 forEach 함수는 배열 내 각각의 인수에 대해 어떠한 로직처리를 할때 사용됩니다.



 출처 : [https://aljjabaegi.tistory.com/314?category=640138]