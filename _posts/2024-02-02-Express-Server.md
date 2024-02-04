---
title: Express Server
date: 2024-02-02 00:00:00 +0900
categories: [JavaScript]
tags: [javascript, express, server]
--- 
- 이번 포스터에서는 `Express Server`에 대해서 알아보고 `CRUD`을 서버에서 구현하는 실습을 진행하고자 한다.

## 1. Express Server란?
- `Express`는 Node.js를 사용하여 쉽게 서버를 구성할 수 있게 만든 클래스와 라이브러리의 집합체이다.
- `Express`는 `프레임워크`이므로 웹 애플리케이션을 만들기 위한 각종 라이브러리와 미들웨어 등이 내장되어 있어 개발하기 편하다는 장점이 존재한다.

## 2. Express 시작하기
- 새로운 express 프로젝트를 생성한다.
```js
npx express-generator --view=ejs <app_name>
```
- 위 명령어로 인해 express server의 기본적인 구성은 완료되었다.
- `package.json`의 `scripts`에 나와있는 대로 `npm run start`을 하게 될 경우에 실행이 된다. 하지만 `start` 상태에서는 코드의 변경사항이 곧바로 확인할 수 없다는 단점이 존재한다.
- 위 문제를 해결하기 위해서 `npm install nodemon`을 실행하고 `package.json` > `scripts`에 `"dev" : "nodemon ./bin/www"`을 추가한 뒤에 `npm run dev`을 통해 실행시켜 준다.
- 이렇게 하게 되면 코드의 변경사항이 곧바로 변경되어서 굳이 서버를 껐다 키는 과정을 막을 수 있다.

## 3. 라우팅
- `라우팅`이란 애플리케이션 엔드 포인트(URL)의 정의, 그리고 URI가 클라이언트 요청에 응답하는 방식을 말한다.
```js
// Get method 예시
app.get('/', (req, res, next) => {
    res.send('GET request to the homepage');
})
```
- `app.js`에서 모든 라우팅을 설정해도 괜찮지만 그렇게 할 경우 가독성이 떨어질 뿐만 아니라 관리하기가 무척 힘들다.
- 따라서 라우터를 만들어서 관련 있는 라우팅끼리 묶어서 관리한다.

  ```js
  // 경로 routes > 라우터_이름.js
  const express = require('express');
  const router = express.Router();

  router.get('/', (req, res, next){

    // req : 요청
    // res : 응답
    // next : 미들웨어

    res.send('This is 라우터_이름.');
  })

  module.exports = router;
  ```
  ```js
  // 경로 app.js
  const 라우터_이름Router = require('./routes/라우터_이름');
  app.use('/라우터_이름', 라우터_이름Router);
  ```

- 이런 방식으로 분리해서 관리한다.

## 4. 예시
- 하나의 예시로 `게시판에서 필요한 CRUD`에 대해 `MongoDB와 연결`해서 구현해 보고자 한다.
- 구현해 보고자 하는 라우팅은 아래와 같다.

  |URL|Method|설명|
  |:---|:---|:---|
  |/board|GET|게시글 리스트를 조회|
  |/board|POST|게시글 생성|
  |/board/:boardId|GET|게시글 자원 중에 boardId에 해당하는 자원만 보여줘|
  |/board/:boardId|PUT|게시글 자원 중에 boardId에 해당하는 자원을 수정해줘|
  |/board/:boardId|DELETE|게시글 자원 중에 boardId에 해당하는 자원을 삭제해줘|

### 4.1. 모델 생성
- MVC 패턴을 위해서 DB와 Connection을 담당하는 Model을 구현하기 위해서 `models`라는 폴더를 추가로 생성해서 그 안에서 `Model`을 정의했다.
  - MVC 패턴이란 Model, View, Controller에 해당하는 기능을 분리해서 구현하는 것이다. 

  ```js
  // 경로 : models > Board.js
  const mongoose = require('mongoose');

  const boardSchema = new mongoose.Schema({
    title : {
      type:String,
      required: true
    },
    content : {
      type : String,
      required: true
    },
    author : String,
    createdAt : {
      type : Date,
      default : Date.now
    }
  })

  const Board = mongoose.model("Board", boardSchema);

  module.exports = Board
  ```

### 4.2. 라우터 연결
- 라우터를 만들기 전에 app.js에서 필요한 설정을 위해 아래 사항을 추가했다.

  ```js
  // 경로 : app.js
  const mongoose = require("mongoose");
  const MONGO_HOST = 'mongodb+srv://<username>:<password>@host/<collectionName>'
  mongoose.connect(MONGO_HOST, {
    retryWrites:true,
    w: 'majority'
  })
  .then(() => console.log("Connected Successful"))
  .catch(err => console.log(err))

  const boardRouter = require('./routes/board');
  app.use('/board', boardRouter);
  ```

### 4.3. 라우터 생성하기
- 게시판 라우팅을 담당하는 파일을 따로 생성해준다.

  ```js
  const express = require('express');
  const router = express.Router();
  const Board = require('../models/Board');

  // 게시글 리스트 조회
  router.get('/', (req, res, next) => {
    Board.find({}).then(data => {
      res.json(data);
    }).catch(err => {
      res.send(err);
    })
  })
  
  // 게시글 생성
  router.post('/', (req, res, next) => {
    Board.create({
      title : req.body.title,
      content : req.body.content,
      author : req.body.author
    }).then(data => {
      res.json(data);
    }).catch(err => {
      res.send(err);
    })
  })

  // 특정 게시글 조회
  router.get('/:boardId', (req, res, next) => {
    Board.findById(req.params.boardId).then(data => {
      res.json(data);
    }).catch(err => {
      res.send(err);
    })
  })

  // 특정 게시글 수정
  router.put('/:boardId', (req, res, next) => {
    Board.updateOne(
      { _id : req.params.boardId},
      { $set : req.body }
    ).then(data => {
      res.json(data);
    }).catch(err => {
      res.send(err);
    })
  })

  // 특정 게시글 삭제
  router.delete('/:boardId', (req, res, next) => {
    Board.deleteOne({
      _id : req.params.boardId
    }).then(data => {
      res.json(data);
    }).catch(err => {
      res.send(err);
    })
  })

  module.exports = router;
  ```

## 마치며
- 서버에 대해서 기초적으로나 다뤄볼 수 있었던 것 같다. Spring을 한 번이라도 다뤄봤고 Javascript에 익숙하다면, express 서버에 대해서 이해하는데 크게 어려움은 없는 것 같다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**