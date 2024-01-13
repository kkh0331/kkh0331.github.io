---
title: AWS ElasticBeanstalk(EB)와 Lambda 연결 실습
date: 2024-01-10 01:00:00 +0900
categories: [AWS 실습]
tags: [aws, ElasticBeanstalk, Lambda, ChatGPT]
---
- [AWS ElasticBeanstalk 기초 실습](/posts/AWS-EB-Practice/)과 [AWS Lambda 기초 실습](/posts/AWS-Lambda-Practice/)에서 했던 내용을 연결하여 ChatGPT와 비슷한 애플리케이션을 만들어 보고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.10` 기준이다.

## 1. EB와 Lambda 연결하기
1. [AWS ElasticBeanstalk 기초 실습](/posts/AWS-EB-Practice/)에서 작업했던 `coredot-chat-demo`에서 `API Gateway`를 호출하기 위해서 아래 명령어를 통해 관련 모듈을 설치한다. 
```ssh
pip install aiohttp
```
2. [여기 주소](https://gist.github.com/koorukuroo/
3a9d96143b93474ef4aeb6efda8bd850)로 들어가서 `application.py` 내용을 업데이트 해준다.
- 업데이트 해준 `application.py`의 `58`라인의 https 주소를 [AWS Lambda 기초 실습](/posts/AWS-Lambda-Practice/)에서 배포했던 api 주소로 수정해 준다.
3. 아래 명령어를 통해 `Local`에서 테스트를 진행해 본다.
```ssh
uvicorn application:app --reload
```
![local에서 테스트](/assets/img/posts/2024-01-10/eb_lambda/test_local.png){: width="90%"}
4. `requirements.txt`에 `aiohttp`, `requests`을 추가해준다.
5. 모든 변경 사항을 github 원격 저장소에 push 해준다.
6. 아래 명령어를 입력해서 지금까지 commit된 사항들을 배포해준다.
```ssh
eb deploy
```
![배포된 url로 테스트](/assets/img/posts/2024-01-10/eb_lambda/test_deploy.png){: width="90%"}

## 마치며
- EB와 Lambda을 연결해 보는 실습을 진행해 보았다.
- 다만, RDS을 연결해주지 않아서 그동안 채팅했던 데이터들을 저장되지 않았던 것이 조금은 아쉬웠다. 다음에 시간이 된다면, RDS을 연결하여 ChatGPT clone 사이트를 제작해서 배포하는 프로젝트를 진행해보고 싶다는 생각이 들었다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**