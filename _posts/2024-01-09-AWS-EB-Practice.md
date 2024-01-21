---
title: AWS ElasticBeanstalk 기초 실습
date: 2024-01-09 00:00:00 +0900
categories: [AWS 실습]
tags: [aws, Elastic, ElasticBeanstalk]
---
- `ElasticBeanstalk(EB)`을 이용해서 채팅 애플리케이션을 배포하는 실습을 진행해 보려고 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.09` 기준이다.

## 1. 실습을 진행하기 전 사전 세팅
![사전 세팅](/assets/img/posts/2024-01-09/pre_setting.png){: width="90%"}
- [AWS Elastic 기초 실습](/posts/AWS-Elastic-Practice/)을 참고하여 사전 세팅을 진행한다.

## 2. Local에서 돌아가는지 확인
1. `ElasticBeanstalk`의 실습이 목적이기 때문에 코드는 직접 작성하지 않고 [CoreDotToday의 coredot-chat-demo](https://github.com/CoreDotToday/coredot-chat-demo)을 활용했다.
```ssh
git clone https://github.com/CoreDotToday/coredot-chat-demo
```
2. 아래 명령어를 통해 `base 가상환경`으로 들어간다.
```ssh
source ~/.bash_profile
```
3. 아래 명령어를 통해 Local에서 실행될 환경을 만들어준다.
```ssh
cd coredot-chat-demo
conda crate -n chat python=3.11 // chat이라는 가상환경 만들고 python 3.11을 설치
conda activate chat // chat 가상환경으로 들어가기
pip install -r requirements.txt // 관련 모듈 설치
uvicorn run:app // Local에서 채팅 애플리케이션 실행
```
4. 실행화면은 아래와 같다.
<p align="middle">
<iframe width="560" height="315" src="https://www.youtube.com/embed/ek2c5Kh8E1c?si=meYHphZ7ig0FUyAP" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</p>

## 3. EB에 채팅 애플리케이션 배포
1. `coredot-chat-demo`에 `.env`파일을 추가하고 아래 환경변수를 추가해준다.
```
ENV_STATE=dev
REDIS_HOST=<ElastiCache의 기본 엔드포인트> //:6379 빼고
```
2. `run.py`를 `application.py`로 수정해준다.
3. `Procfile`를 추가해주고 아래 사항을 추가해준다.
```
web: uvicorn application:app --port=8000 --host=0.0.0.0 --proxy-headers --forwarded-allow-ips='*'
```
4. 아래 명령어를 통해 git 설정을 진행해 준다.
```ssh
rm -rf .git // clone한 곳의 정보를 가지고 있는 .git을 삭제
git init
git remote add orign <git repository 주소>
git branch -M main
git add .
git commit -m 'Set init setting'
git push origin main
```
5. AWS의 `콘솔 홈 화면`에서 `Elastic Beanstalk`을 입력하고, 검색 결과를 클릭한다.
6. 화면 좌측에 있는 탭에서 `애플리케이션`를 클릭하고 `애플리케이션 생성` 버튼을 클릭한다.
- 원하는 `애플리케이션 이름`을 입력하고 `생성` 버튼을 누른다.
7. 생성된 `애플리케이션`의 환경 세팅을 실시한다.
![환경 세팅 01](/assets/img/posts/2024-01-09/set_env_setting_01.png){: width="90%"}
`플랫폼`에서 `Python`을 클릭하고 `사전 설정`에서 `고가용성`을 클릭하고 나머지는 기본 설정으로 세팅하고 `다음` 버튼을 누른다.
![환경 세팅 02](/assets/img/posts/2024-01-09/set_env_setting_02.png){: width="90%"}
![환경 세팅 03](/assets/img/posts/2024-01-09/set_env_setting_03.png){: width="90%"}
나머지는 기본 설정으로 세팅하고 `환경 속성`이 나올 때까지 계속 넘어가준다.
![환경 세팅 04](/assets/img/posts/2024-01-09/set_env_setting_04.png){: width="90%"}
8. 생성된 도메인으로 접속해주면 아래 화면과 같다.
![Sample 웹 사이트](/assets/img/posts/2024-01-09/sample_web_site.png){: width="90%"}
9. 아래 명령어를 통해서 `AWS에서 만들어 놓은 것`과 [`coredot-chat-demo`](#2-local에서-돌아가는지-확인)을 연결해준다.
```ssh
eb init
```
- `Select a default region` -> `10`
- `Select an application to use` -> 아까 생성한 `애플리케이션 이름`을 선택
- `Do you wish to continue with CodeCommit? (Y/n)` -> `n`
10. 명령어를 통해서 배포해준다.
```ssh
eb deploy
```
11. 실행화면은 아래와 같다.
<p align="middle">
<iframe width="560" height="315" src="https://www.youtube.com/embed/a5zFFnPoxKE?si=_l_Zf8F0KkZMrF7L" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</p>
- 위의 `Local`의 실행화면과 비교해보면 주소가 다른 것을 확인할 수 있다.

## 마치며
- Elastic Beanstalk을 활용해서 간단한 채팅 애플리케이션을 배포해 보는 실습을 진행해 보았다.
- 추후에 python으로 이루어진 채팅 애플리케이션 코드를 자바 언어로 전환해 볼 예정이다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**