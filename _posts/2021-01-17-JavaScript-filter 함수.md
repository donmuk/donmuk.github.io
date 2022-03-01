---
layout: post
title: "자바스크립트 filter 함수"

comments: true
description: ""
keywords: ""
tags: [JAVASCRIPT]
---

javascript filter 함수에 대해 알아보자 자바스크립트 filter 함수

filter 함수는 명칭과 같이 callbackFunction의 조건에 해당하는 모든 요소가 있는 배열을

새로 생성하는 기능을 합니다.

filter 함수의 구문은 아래와 같습니다.

```
var newArray = arr.filter(callbackFunction(element, index, array), thisArg);
```

filter 함수의 매개변수는 callbackFunction 과 thisArg 입니다.

callbackFunction에는 3개의 매개변수를 사용할 수 있습니다.

element : 요소값

index : 요소의 인덱스

array : 사용되는 배열 객체

 

그리고 thisArg 는 filter에서 사용될 this 값입니다. 선택적으로 사용되며 사용하지 않을 경우

undefined 전달 됩니다.

그럼 예제를 보도록 하죠.

```
<script type="text/javascript">
(function test(){
var testArray = [1,2,3,4,5];
var newArray = testArray.filter(function(element, index, array){
    console.log(element);
    console.log(index);
    console.log(array);
    });
})(); 
</script>
```

크롬에서 실행해 보면 아래와 같은 결과를 얻습니다.

 ![654683241536](/images/javascript/654683241536.jpg)


위의 설명과 같이 element 에는 현재 순회하는 배열의 인자값,

index에는 그 인자값의 인덱스,

array에는 현재 배열이 로그창에 찍히게 됩니다.


그럼 이제 filter란 함수가 어떻게 동작하는지 알았으니 응용을 해보도록 하죠.

위의 소스에 배열이 3이하인 값만 추출해 새 배열을 만들어 보도록 하겠습니다.

```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5];
    var newArray = testArray.filter(function(element){
        return element<=3;
    });
    console.log(newArray);
})(); 
</script>
``` 

결과는 [1,2,3]

아주 간단하죠? 현재 인자값인 element가 3 이하인 것만 리턴을 하면 됩니다.

보통은 아래와 같이 코딩하죠. 조건에 해당하는 callbackFunction을 따로 구현 합니다.

 
```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5];
    function getThreeUpper(value){
        return value>=3;
    }
    var newArray = testArray.filter(getThreeUpper);
    console.log(newArray);
})(); 
</script>
``` 

결과는 위와 같습니다.

이처럼 filter 함수를 활용해 배열에서 조건에 해당하는 값만을 추출해 낼 수 있습니다.

좀 더 이해를 돕기 위해 다른 예를 보도록 하겠습니다.

json 객체에서도 filter를 활용할 수 있는데요.

json 객체내에서 조건에 맞는 값을 찾거나 유효한 데이터만을 추출 할 수도 있습니다.

```
<script type="text/javascript">
(function test(){
    var testJson = [{name : "이건", salary : 50000000},
                    {name : "홍길동", salary : 1000000},
                    {name : "임신구", salary : 3000000},
                    {name : "이승룡", salary : 2000000}];
    
    var newJson = testJson.filter(function(element){
        console.log(element);
        return element.name == "이건";
    });
    console.log("newObj");
    console.log(newJson);
})(); 
</script>
```
 

testJson에서 name 이 "이건" 인 사람을 찾을 때의 코드 입니다.

결과는 아래와 같습니다.

![687654654313](/images/javascript/687654654313.jpg)


element에는 jsonArray의 jsonObject들이 넘어오게 되고, 그중에 name이 "이건" 인 Object를 찾아 리턴합니다.

그럼 salary가 3000000 이상인 사람은 어떻게 할까요?

 
return element.salary>=3000000; 을 하면 되겠죠? ( <- 드래그 하시면 보입니다.)

json 데이터의 유효성 검증에도 활용할 수 있습니다.

```
<script type="text/javascript">
(function test(){
    var testJson = [{name : "이건", salary : 50000000},
                    {name : "홍길동", salary : 1000000},
                    {name : "임신구", salary : 3000000},
                    {name : "이승룡", salary : 2000000},
                    {},
                    {name : "이철수", salary : NaN},
                    {name : "이재춘", salary : 'undefined'},
                    {name : "이제일", salary : -2000000}];
 
    function numberFilter(obj){
        if ('salary' in obj && (typeof(obj.salary)==='number') && !isNaN(obj.salary) && obj.salary>0){
            return true;
        }else{
            return false;
        }
    }
    var newJson = testJson.filter(numberFilter);
    console.log("newObj");
    console.log(newJson);
})(); 
</script>
```
 

참고) 

[in연산자]

속성 in 객체명

객체에 속성이 있으면 true, 없으면 false

[isNaN 함수]

isNaN(value)

value 값이 NaN(Not a Number : 숫자가 아니다) 인지 확인.

isNaN(123); ==> false

isNaN((-1.23); ==> false

isNaN(5-2); ==> false

isNaN(0); ==> false

isNaN('123'); ==>false

isNaN('Hello'); ==> true

isNaN('2005/12/12'); ==> true

isNaN(''); ==> false

isNaN(true); ==> false;

isNaN(undefined); ==> true

isNaN('NaN'); ==> true

isNaN(NaN); ==> true

isNaN(0/0); ==> true

'salary' in obj 에서 'salary'가 없는 {} 가 걸러지고,

typeof(obj.salary)==='number' 에서 이재춘이 걸러지고

!isNaN(obj.salary) 에서 이철수가 걸러지며

obj.salary>0 에서 이제일이 걸러지니

 
결과는 아래와 같게 되죠.

![687654313245736](/images/javascript/687654313245736.jpg)

이처럼 filter 함수는 배열이나 JSON 객체내의 조건에 해당하는 값만을 추출할때 유용하게 사용됩니다.

그럼 thisArg 매개변수의 사용법을 보도록 하겠습니다.

예를 들어 배열에서 1부터 10 사이의 수만 추출한다고 합시다.

위의 코드를 참고하여 보면

```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5,50,100];
    function getThreeUpper(value){
        return value>=1 && value<=10;
    }
    var newArray = testArray.filter(getThreeUpper);
    console.log(newArray);
})(); 
</script>
```
 
이 되겠죠. 여기에 thisArg 매개변수를 활용해 보도록 하겠습니다.

 ```
<script type="text/javascript">
(function test(){
    var testArray = [1,2,3,4,5,50,100];
    var obj = {min : 1, max : 10};
    function getThreeUpper(value){
        return value>=this.min && value<=this.max;
    }
    var newArray = testArray.filter(getThreeUpper, obj);
    console.log(newArray);
})(); 
</script>
``` 

thisArg로 obj를 넘겼고, 이것을 callbackFunction 내에서 this로 활용하였습니다.

filter 함수의 사용법에 대해 알아보았는데요. 이런 고차함수들을 사용하지 않고 구현하게 되면

소스가 상당히 늘어나게 되겠죠 ㅎ

확실히 이해하여 효율적인 코딩 합시다~

 

 출처 : [https://aljjabaegi.tistory.com/312]