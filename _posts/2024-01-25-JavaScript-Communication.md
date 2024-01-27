---
title: 프론트엔드에서의 네트워크 통신
date: 2024-01-25 00:00:00 +0900
categories: [JavaScript]
tags: [javascript, react, fetch, axios]
--- 
- 이번 포스터에서는 `JavaScript` 및 `React`에서 `fetch`와 `axios`을 이용한 네트워크 통신에 대해서 학습하고자 한다.

## 1. 네트워크 통신이란?
- `클라이언트`는 `서버`에게 데이터를 요청하고 `서버`는 이에 응답하여 데이터를 보내준다.
- 요청(request)에는 기본적으로 4가지 method 존재한다.
  - GET(조회), POST(넣기), PUT(업데이트), DELETE(삭제)

## 2. Fetch
- 웹에서 데이터를 가져오거나 보내기 위한 JavaScript의 내장 함수이다.
- `Promise` 기반으로 HTTP를 이용해서 서버와 비동기로 통신한다.
- `fetch()` 함수는 첫 번째 인자로 **URL**, 두 번째 인자로 **옵션 객체**를 받고 **Promise 타입의 객체**를 반환한다.

### 2.1. GET Method
- 일반적으로 **옵션 객체** 안에 요청 메소드 타입을 넣어야 되지만, 넣지 않을 경우 `GET`으로 여겨진다.
- `aynsc/await` 이용 X

  ```js
  fetch('https://jsonplaceholder.typicode.com/posts/1')
    .then((response) => response.json())
    .then((json) => console.log(json))
    .catch((error) => console.error("Error : "),error);
  ```

- `aynsc/await` 이용

  ```js
  async function getFetch(){
    try{
        const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
        const data = await response.json();
        console.log(data);
    }catch(error){
        console.error("Error : ",error);
    }
  }

  getFetch();
  ```

### 2.2. POST Method
- `aynsc/await` 이용 X

  ```js
  fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      title: 'foo',
      body: 'bar',
      userId: 1,
    }),
    headers: {
      'Content-type': 'application/json; charset=UTF-8',
    },
  })
    .then((response) => response.json())
    .then((json) => console.log(json))
    .catch((error) => console.error("Error : "),error);
  ```

- `aynsc/await` 이용

  ```js
  async function postFetch(){
    try{
      const response = await fetch('https://jsonplaceholder.typicode.com/posts',{
        method: 'POST',
        body: JSON.stringify({
          title: 'foo',
          body: 'bar',
          userId: 1,
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },   
      });
      const data = await response.json();
      console.log(data);
    }catch(error){
      console.error("Error : ",error);
    }
  }

  postFetch();
  ```

### 2.3. PUT Method
- `aynsc/await` 이용 X

  ```js
  fetch('https://jsonplaceholder.typicode.com/posts/1', {
    method: 'PUT',
    body: JSON.stringify({
      id: 1,
      title: 'foo',
      body: 'bar',
      userId: 1,
    }),
    headers: {
      'Content-type': 'application/json; charset=UTF-8',
    },
  })
    .then((response) => response.json())
    .then((json) => console.log(json))
    .catch((error) => console.error("Error : "),error);
  ```

- `aynsc/await` 이용

  ```js
  async function putFetch(){
    try{
      const response = await fetch('https://jsonplaceholder.typicode.com/posts/1',{
        method: 'PUT',
        body: JSON.stringify({
          id: 1,
          title: 'foo',
          body: 'bar',
          userId: 1,
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      });
      const data = await response.json();
      console.log(data);
    }catch(error){
      console.error("Error : ",error);
    }
  } 

  putFetch();
  ```

### 2.4. DELETE Method
- `aynsc/await` 이용 X

  ```js
  fetch('https://jsonplaceholder.typicode.com/posts/1', {
    method: 'DELETE',
  })
    .then((response) => console.log(response.status));
    .catch((error) => console.error("Error : "),error);
  ```

- `aynsc/await` 이용

  ```js
  async function deleteFetch(){
    try{
      const response = await fetch('https://jsonplaceholder.typicode.com/posts/1',{
        method: 'DELETE'  
      })
      const status = response.status;
      console.log(status);
    }catch(error){
      console.error("Error : ",error)
    }
  }

  deleteFetch();
  ```

## 3. Axios
- node.js와 브라우저를 위한 `Promise` 기반 HTTP 클라이언트이다.

### 3.1. GET Method
- `aynsc/await` 이용 X

  ```js
  const url = 'https://jsonplaceholder.typicode.com/posts/1';
  axios.get(url)
    .then(resp => resp.data)
    .then(data => console.log(data))
    .catch(err => console.error('Error : ',err))
  ```

- `aynsc/await` 이용

  ```js
  async function getAxios(){
    try{
      const resp = await axios(url);
      console.log(resp.data);
    }catch(err){
      console.error("Error : ", err);
    }
  }

  getAxios();
  ```

### 3.2. POST Method
- `aynsc/await` 이용 X

  ```js
  const url = 'https://jsonplaceholder.typicode.com/posts';
  const data = {
      title: 'foo',
      body: 'bar',
      userId: 1,
  }
  axios.post(url, data)
    .then(resp => resp.data)
    .then(data => console.log(data))
    .catch(err => console.error('Error : ',err))
  ```

- `aynsc/await` 이용

  ```js
  async function postAxios(){
    try{
      const data = {
        title: 'foo',
        body: 'bar',
        userId: 1,
      };
      const resp = await axios.post(url, data);
      console.log(resp.data);
    } catch (err){
      console.error('Error : ',err);
    }
  }

  postAxios();
  ```

### 3.3. PUT Method
- `aynsc/await` 이용 X

  ```js
  const url = 'https://jsonplaceholder.typicode.com/posts/1';
  const data = {
    id: 1,
    title: 'foo',
    body: 'bar',
    userId: 1,  
  }
  axios.put(url, data)
    .then(resp => resp.data)
    .then(data => console.log(data))
    .catch(err => console.error('Error : ', err))
  ```

- `aynsc/await` 이용

  ```js
  async function putAxios(){
    try{
      const data = {
        id: 1,
        title: 'foo',
        body: 'bar',
        userId: 1,
      };
      const resp = await axios.put(url, data);
      console.log(resp.data);
    } catch (err){
      console.error('Error : ',err);
    }
  }

  putAxios();
  ```

### 3.4. DELETE Method
- `aynsc/await` 이용 X

  ```js
  const url = 'https://jsonplaceholder.typicode.com/posts/1';
  axios.delete(url)
    .then(res => console.log(res.status))
    .catch(err => console.err('Error : ',err))
  ```

- `aynsc/await` 이용

  ```js
  async function deleteAxios(){
    try{
      const resp = await axios.delete(url);
      console.log(resp.status);
    }catch(err){
      console.err('Error : ', err)
    }
  }

  deleteAxios();
  ```

## 4. fetch와 axios 중에서 선택
- 위에서 살펴본 바와 같이 전체적으로 `fetch`와 `axios`는 전체적으로 비슷한 것 같다. 하지만 약간의 차이점이 존재하는 것 같다.
- 차이점 1 : 데이터 넘기는 방법
  - axios : 객체 형태로 넘김
  - fetch : string화 해서 넘김
- 차이점 2 : response 얻는 방법
  - axios : response 객체의 data property에 접근하여 얻음
  - fetch : response 객체에 .json() 메소드를 호출하여서 json 객체를 얻음
- 차이점 3 : 사용하기 위해서
  - axios : 라이브러리 형태이다. 따라서 JavaScript나 React의 버전이 업그레이드될 경우 기존의 axios 버전이 지원을 하지 않을 수 있다는 문제점이 존재하는 것 같다.
  - fetch : 내장 함수이다.
- 물론 어느정도 사람마다 차이는 존재하겠지만, 내가 보았을 때는 `axios`의 코드가 더 간단하고 가독성이 좋은 것 같다. -> 앞으로는 `axios`을 더 자주 사용할 것 같다...

## 마치며
- 네트워크 통신에 대해서 자세히 공부하고 정리할 수 있는 시간이었다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**