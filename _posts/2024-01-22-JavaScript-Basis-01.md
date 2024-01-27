---
title: JavaScript 기초 01
date: 2024-01-22 00:00:00 +0900
categories: [JavaScript]
tags: [javascript]
--- 
- 이번 포스터에서는 `JavaScript`의 기본 개념과 문법에 대해서 알아보고자 한다.

## 1. 변수
- `var` : 변수 선언(값 변경 가능)
- `let` : 변수 선언(값 변경 가능)
- `const` : 상수 선언(값 변경 불가)
- `var`을 사용하게 될 경우, 코드 실행할 때 선언을 함수 범위의 최상단으로 올라가 버려서, <span style="color:red">오류가 나야할 때 나지 않은 문제</span>가 발생
- 되도록이면, `let`과 `const` 사용하자!!

## 2. 원시 자료형(Primitive DataType)
- **typeof(값)**을 통해 자료형 확인 가능
- `String` : 문자열
- `Number` : 숫자형 → 정수, 실수 구분 안함
- `Boolean` : true/false
- `object` : 객체
- `BigInt` : BigInt 값
  - 정수 뒤에 n을 붙여야 함
  - typeof(111n) → bigint
- `array` : 배열
  - 특수한 형태의 object
  - typeof([1,2,3]) → object
  - Array.isArray([1,2,3]) → true
- `null` : 비어 있는 값
  - But, typeof(null) → object → 호환성 문제를 위해서
- `undefined` : 정의되지 않은 값

## 3. 연산자
### 3.1. 산술 연산자
- `+` : 덧셈
- `-` : 뺄셈
- `*` : 곱셈
- `/` : 나눗셈
  - `정수/정수 = 소수`가 될 수 있음
- `**` : 거듭제곱
- `%` : 나머지
- `++` : 1씩 더하기
- `--` : 1씩 빼기

### 3.2. 비교 연산자
- `<` : 작다
- `<=` : 작거나 같다
- `>` : 크다
- `>=` : 크거나 같다
- `==` `===` : 같다 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `!=` `!==` : 같지 않다
  - `==` `!=`은 데이터형을 반영하지 않고 비교
  - `===` `!==`은 데이터형을 반영하여 비교 → <span style="color:red">선호!!</span>

### 3.3. 논리 연산자
- `||` : OR 연산자
- `&&` : AND 연산자
- `!` : NOT(반대)

## 4. 문자열
- 큰 따옴표("), 작은 따옴표(') 구분하지 않음
- 백틱(`)을 사용하면 <span style="color:red">문자열 내에서 변수</span> 사용 가능
```js
let name = "홍길동";
let multi = `Hello ${name}. Hello, JS`;
```

### 4.1. 문자열 관련 함수 및 속성

|함수|설명|
|:---|:---|
|.length|문자열의 길이 반환|
|.charAt(indexing)/at(indexing)|index에 해당하는 문자 반환|
|.toUpperCase()|대문자로 변경|
|.toLowerCase()|소문자로 변경|
|.indexOf(char)|찾는 문자의 index 반환. 없을시 -1 반환|
|.includes(str)|문자열이 포함되어 있는지 여부(true/false 반환)|
|.startsWith(str)|주어진 str으로 시작하는지 여부|
|.endsWith(str)|주어진 str으로 끝나는지 여부|
|.slice(start [, end])|문자열 자르기|
|.split(token)|토근을 기준으로 문자열 쪼개기. 배열 반환|
|.trim()|양쪽 공백 제거|
|.repeat(n)|문자열 반복|

## 5. 함수
- 기본 함수 형태
```js
function min(a,b){
    return a < b ? a : b;
}
```
- 함수 표현식과 화살표 함수

  ```js
  const min = function(a,b){
    return a < b ? a : b;
  }

  const min = (a, b) => {
    return a < b ? a : b;
  }

  // 함수 부분이 바로 return이 나오는 경우 아래와 같이 표현 가능
  const min = (a, b) => (a < b ? a : b);
  ```

- 위의 4가지 형태를 자유롭게 쓸 수 있어야 함.

## 6. 배열
- 배열 요소 간에 굳이 동일할 필요 없음
```js
let sampel = [1, 2, 3, "a", "b", "c"]
```

### 6.1. 배열 관련 함수 및 속성

|함수|설명|
|:---|:---|
|.length|길이 반환|
|.at(indexing)/[indexing]|index에 해당하는 배열 요소 반환|
|.indexOf(char)|찾는 요소의 index 반환. 없을 시 -1 반환|
|.includes(elem)|요소가 포함되어 있는 여부(true/false 반환)|
|.slice(start [, end])|배열 슬라이싱|
|.join(token)|토큰을 기준으로 요소들을 문자열로 만들기|
|.reverse()| 순서 뒤집기|
|push(elem), pop()|요소 마지막에 추가하기, 마지막 요소 꺼내기|
|splice(start, deleteCount, ...items)|start index부터 deleteCount만큼 지운 후 items를 채워넣음|

### 6.2. 배열 관련 함수 및 속성 - <span style="color:red">중요</span>
- forEach(callbackFn) : 배열의 요소를 반복돌면서, 작업을 진행

  ```js
  let sample = [1, 2, 3, "a", "b", "c"];

  sample.forEach(function(item, index, array){
      console.log(item);
      console.log(index);
      console.log(array);
      console.log('-'.repeat(10))
  })
  ```

- map(callbackFn) : 배열의 요소들을 모두 순회하여 callback 함수의 **return** 값을 모아 **새로운 배열**을 만듬

  ```js
  let sample = [1, 2, 3];

  let sample2 = sample.map(function(value, index, array){
      return value * 10;
  })

  console.log(sample2); // [10, 20, 30]
  ```

- filter(callbackFn) : 배열의 요소를 반복 돌면서, true인 요소들만 모아 **새로운 배열**을 만듬

  ```js
  let sample = [1, 2, 3, 4, 5];

  let sample2 = sample.filter(function(value, index, array){
      if(value > 3.5){
          return true;
      } else {
          return false;
      }
  })

  console.log(sample2); //[4, 5]
  ```

- concat(...items) : 배열을 합쳐서 사용함

  ```js
  let sample = [1, 2, 3, 4, 5];
  let sample2 = sample.concat(6, 7, 8);

  console.log(sample2); // [1, 2, 3, 4, 5, 6, 7, 8]
  ```

- reduce(callbackFn, initialValue) : 배열의 요소들을 모두 순회하여 initialValue부터 시작하여 누적된 결과를 만듬

  ```js
  let sample = [1, 3, 5, 7, 9];

  let sample2 = sample.reduce(function(prev, cur, curIdx, array){
      console.log(prev);
      console.log(cur);
      console.log('------');
      return cur + prev;
  }, 0)

  console.log(sample2);
  ```

## 7. 조건문
### 7.1. if 문
```js
if(조건식){
    실행문;
} else if(조건식 2){
    if(조건식 3){ //중첩 if문 가능
        실행문;
    } else {
        실행문;
    }
} else {
    실행문;
}
```
### 7.2. 물음표 연산자
```js
(조건식) ? (true시 값) : (false시 값);
```
### 7.3. switch case

```js
let 변수 = 초깃값;

switch(변수){
    case 값 1:
        실행문 1;
        break;
    case 값 2:
        실행문 2;
        break;
    default:
        실행문 3;
}
```

## 8. 반복문
### 8.1. for문

```js
// for문의 형태(1)
for(begin;condition;step){
  // ... 반복문 본문 ...
}

// for문의 형태(2)
for(let item in Object){
  // ... 반복문 본문 ...
  // item은 key값이 들어가게 됨. 배열인 경우 index 들어감
}

// for문의 형태(3)
for(let item of Array){
  // ... 반복문 본문 ...
  // item은 배열 index에 해당하는 value들이 들어감.
}
```

### 8.2. while문

```js
let 변수 = 초깃값;

while(조건){
  실행문;
}
```

### 8.3. do-while문

```js
let 변수 = 초깃값;

do{
  실행문
} while(조건);
```

### 8.4. 반복문 제어
- **continue** : 반복문 내에 뒤 코드는 진행하지 않고 반복문 초기(조건검사)로 다시 돌아가서 실행
- **break** : break를 사용하면 그 시점에서 바로 반복문 빠져나옴

## 9. 객체(Object)
### 9.1. JS 객체의 형태

```js
const KEY = "SAMPLE"
let johnProfile = {
  name : "John",
  sayHi : function(){
    console.log(this.name, " 친구야 반갑다!");
  },
  [KEY]: "sampleValue",
};

console.log(obj1); // { name: 'John', sayHi: [Function: sayHi], SAMPLE: 'sampleValue' }
console.log(obj1.name); // John
console.log(obj1['name']); // John
obj1.sayHi(); // John 친구야 반갑다.

console.log(obj1.sampleKey); // undefined
console.log(obj1[KEY]); // sampleValue
console.log(obj1.SAMPLE); // sampleValue
```

### 9.2. 구조 분해 및 할당
- 위치에 맞춰서 각각 할당이 됨

  ```js
  // 위치에 맞춰서 각각 할당이 됨
  let arr = ["Kim", "Shin"];
  let [name1, name2] = arr;

  console.log(name1); // Kim
  console.log(name2); // Shin
  ```

- `...`을 사용할 경우, 나머지 요소들을 전부 할당 받음
```js
let arr2 = [1,2,3,4];
let [v1, v2, ...v3] = arr2;
console.log(v1); // 1
console.log(v2); // 2
console.log(v3); // [3, 4]
```
- 객체의 키 값과 일치하는 변수에 해당 value가 매핑됨

  ```js
  let opions = {
      title : "Menu",
      width : 100,
      height : 200,
      k1 : 'v1',
      k2 : 'v2',
      text : 'sys'
  }

  let {height, title} = opions;
  console.log(height); // 200
  console.log(title); // Menu
  ```

- 객체는 함수 내의 인자로 사용 가능 -> 인자로 받은 opions과 일치하는 키값은 할당이 되고 나머지는 r에 객체 형태로 할당

  ```js
  function func1({width, height, ...r}){
      console.log(width);
      console.log(height);
      console.log(r);
  }

  func1(opions);
  /*
  100
  200
  { title: 'Menu', k1: 'v1', k2: 'v2', text: 'sys' }
  */
  ```

- 가변 인자 가능 -> 사용자가 원하는대로 삽입해라

  ```js
  function func2(num1, ...num2){
      console.log(num1);
      console.log(num2);
  }

  func2(1,2,3,4,5);
  // 1
  // [ 2, 3, 4, 5 ]
  ```

- `...`으로 인해 opions 객체 안의 요소들이 풀어서 넣게됨. text만 'ys'로 변경됨
```js
const obj2 = {
    ...opions, //obj1이 복사되어 들어온다.
    text : 'ys'
}
```

## 10. 예외처리
- 혹시 모를 에러를 방지하기 위해서 사용함

  ```js
  try{
    // 코드 진행
  } catch(err){
    // 에러 핸들링
  } finally{
    // 무조건 실행됨
  }
  ```

## 마치며
- 앞으로 React을 통해 개발을 할 때, [배열 관련 함수 및 속성](#62-배열-관련-함수-및-속성---중요) 과 `화살표 함수`를 많이 사용할 것이라고 느꼈다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**