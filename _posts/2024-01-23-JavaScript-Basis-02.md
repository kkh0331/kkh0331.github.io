---
title: JavaScript 기초 02
date: 2024-01-23 00:00:00 +0900
categories: [JavaScript]
tags: [javascript, asynchrouns, promise, await]
--- 
- 이번 포스터에서는 `JavaScript`의 비동기 & 동기에 대해서 학습하고자 한다.

## 1. 비동기 프로그래밍이란?
- `비동기 프로그래밍`은 여러 작업들이 존재할 때, 하나의 작업이 완료될 때까지 기다리지 않고 다른 작업도 수행하는 기술을 말한다.
- `동기 프로그래밍`에서는 `A, B, C`라는 작업이 있으면 순차적으로 실행이 된다. 하지만 `A`가 굉장히 오래 걸린다면, `B와 C`가 실행될 때까지 오랜 시간이 걸린다.
  - 여기에서 오래 걸리는 작업이라고 한다면, `데이터를 요청하고 받는 통신`과 같은 일을 말한다.
- `비동기 프로그래밍`에서는 `A`의 작업(시간이 오래 걸림)이 완료되기 전에 `B와 C`가 실행이 된다.
- 하지만 만약에 `A`의 작업 결과가 `B와 C`에도 영향을 미치는 코드라고 했을 때, `비동기 프로그래밍`에서는 문제가 발생한다. 

## 2. JavaScript에서 비동기 문제를 해결한 방식
### 2.1. 전통적인 방식(초기)
- `callback function`을 이용한다.
- `callback function`은 JavaScript의 특성을 이용해 다른 함수에게 인자로 전달되는 함수를 가리킨다.
- 아래 코드를 살펴봅시다.

  ```js
  function example(){
    let data;

    setTimeout(()=>{
        data = 100;
    }, 3000);

    return data
  }

  function main(){
    let value = example();
    value *= 2;
    console.log(`value : ${value}`);
  }

  main();
  ```

  - 바라는 결과는 3초 이후에 `value : 200`이라고 출력이 되어야 한다. 하지만 `비동기 프로그래밍`으로 인해 `value : NaN`을 출력이 되고 3초 이후에 끝난다.

- 이를 `callback function`을 이용해 해결하면 다음과 같다.

  ```js
  function example(callback){
    setTimeout(()=>{
        const data = 100;
        callback(data);
    }, 3000);
  }

  function main(){
    example((data) => {
        const value = data * 2;
        console.log(`value : ${value}`);
    })
  }
  ```

  - 위와 같은 방식으로 변경하면, 3초 이후에 `value : 200`이 출력되는 것을 확인할 수 있다.
- 이러한 방식으로 초기에는 `비동기적 프로그래밍`의 문제점을 해결했다.
- 하지만, callback function이 하나가 아닌 이어지는 경우 코드가 깊어지는 `가독성`이 나빠지는 단점이 존재했다.(<span color="red">콜백지옥</span>)

### 2.2. 더 나은 방식 - Promise 객체
- `callback function`의 문제점을 해결하기 위해 나온 방식이다.
- `Promise` 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타낸다.
- `Promise`을 활용하는 방식은 아래와 같다.

  ```js
  // Promise는 resolve와 reject라는 함수형 파라미터를 가진다.
  const promise = new Promise(function(resolve, reject){
    //Promise을 통해 넘겨주고 싶은 값을 resolve에 담는다.
    resolve("성공했을 때 넘겨주고 싶은 값") //추후에 then으로 받을 수 있는 값
    //실패나 에러일 경우에는 reject을 통해 넘겨준다.
    reject("실패했을 때 넘겨주고 싶은 값") // 추후에 catch로 받을 수 있는 값
  })

  // Promise을 사용할 때
  function returnPromise(){
    return new Promise((resolve, reject) => { ... })
  }

  // 가져올 때
  returnPromis()
    .then((result) => console.log("성공 : ", result)) // Promise을 정의할 대 resolve 함수의 인자로 넘겨준 값을 result로 받게됨.
    .catch ((error) => console.log("실패 : ", error)) // Promise을 정의할 때 reject 함수의 인자로 넘겨준 값을 error로 받게됨.
  ```

- [2.1. 전통적인 방식(초기)](#21-전통적인-방식초기)에서 있었던 문제의 코드를 `Promise`을 적용해서 바꾸면 아래와 같다.

  ```js
  function example() {
    return new Promise((res, rej) => {
      let data;
      setTimeout(() => {
        data = 100;
        res(data);
      }, 3000);
    })
  }

  function main() {
    example()
      .then((result) => console.log(`value : ${result * 2}`))
    }

  main();
  ```

- `then`의 경우에는 연속적으로 사용할 수 있는데 `then`의 return으로 넘긴 값이 다음 `then`의 인자 값이 된다.
- 하지만 이 방식도 함수가 이어질 경우에는 `then`인 연속적으로 나오는 `then` 지옥에 빠질 수 있다.

### 2.3. 가장 최근 방식 - Async & Await
- 비동기적인 코드를 함수의 문법으로 추가한 경우이다.
- 즉, 마치 동기적 코드처럼 비동기 함수를 작성하는 방식이다.
- `await`인 경우에는 `Promise` 객체 앞에 작성할 수 있으며, 의미는 `Promise`가 완료될 때까지 **기다리라**는 의미이다.
- 함수 내에 `await`을 사용했으면 필연적으로 그 함수에는 `async`을 붙여야 한다.
- [2.1. 전통적인 방식(초기)](#21-전통적인-방식초기)에서 있었던 문제의 코드를 `async`와 `await`을 이용해 적용해 보면 아래와 같다.

  ```js
  function example() {
    return new Promise((res, rej) => {
      let data;
      setTimeout(() => {
        data = 100;
        res(data);
      }, 3000);
    })
  }

  async function main() {
    const data = await example();
    const value = 2 * data;
    console.log(`value : ${value}`);
  }

  main();
  ```

- `main()` 함수만 놓고 비교해 보면 코드가 훨씬 간단해져서 가독성이 높아진 것을 확인할 수 있다.

## 마치며
- `Promise`, `await`나 `async` 방식은 지금도 주로 쓰이는 방식이는 만큼 확실히 정리할 수 있게 되었던 것 같다.
- 특히, 추후 `fetch`나 `axios`을 이용해서 통신할 때 도움이 많이 될 것 같다.

## 참고 자료
> **[mdn web docs의 JavaScript의 비동기성](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous)**<br/>
> **신한투자증권 프로 디지털 아카데미 3기**