---
title: AWS Elastic 기초 실습
date: 2024-01-08 03:00:00 +0900
categories: [AWS 실습]
tags: [aws, Elastic]
---
- `Elastic 생성`하고 `iTerm`과 `redisinsight`을 활용해서 기존에 생성해둔 ec2를 통해 `Elastic`을 연결해 보는 실습을 진행하고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.05` 기준이다.

## 1. Elastic 생성
1. 사용자 계정으로 로그인하면 `콘솔 홈 화면`으로 들어와 진다. 
- 루트 사용자의 안전을 보장하기 위해서 새로 만든 사용자 계정에서 실시하려고 한다.
2. 먼저, `VPC` 안에 2개의 가용영역을 할당했고 각 가용영역 안에 서브넷을 만들었다. 그런 다음에 `SSH Tunneling`을 진행하기 위해서 한 서브넷에 EC2를 생성했다.([EC2 실습](/posts/AWS-EC2-Practice/)이랑 [VPC 실습](/posts/AWS-VPC-Practice/) 참고)
3. 검색창에 `Elastic`를 입력하고, 검색 결과를 클릭한다.
4. 화면 좌측에 있는 탭에서 `Redis 캐시`를 클릭해서 Redis 캐시 관리 페이지로 이동하여 `Redis 캐시 생성` 버튼을 클릭한다.
![Redis 캐시 생성 버튼 클릭](/assets/img/posts/2024-01-08/elastic/click_create_redis_cache_btn.png){: width="90%"}
5. 아래와 같이 세팅한다.
![Redis 캐시 생성 세팅](/assets/img/posts/2024-01-08/elastic/set_redis_cache.png){: width="90%"}
`클러스터 정보`에서 `이름`과 `설명(선택사항)`을 입력한다.
![Redis 캐시 생성 세팅 2](/assets/img/posts/2024-01-08/elastic/set_redis_cache_02.png){: width="90%"}
`클러스터 설정`에서는 아무것도 건드리지 않아도 무방하나 여기에서는 `노드 유형`만 `cache.t4g.micr0`로 변경했다.
![Redis 캐시 생성 세팅 3](/assets/img/posts/2024-01-08/elastic/set_redis_cache_03.png){: width="90%"}
추가적으로 `연결`에서 `서브넷 선택됨` 부분이 있는데 이 서브넷이 `Public 서브넷`인지 확인하고 아닐 경우에는 제외시켰다.
나머지는 전부 `default`로 설정했다.
6. 생성 후 보안 그룹 설정하기 위해서 검색창에 `EC2`를 입력하고, 검색 결과를 클릭한 후, 화면 좌측 탭에서 `네트워크 및 보안`에서 `보안 그룹`을 선택하고 `보안 그룹 생성` 버튼을 클릭해서 관련 내용을 세팅한다.
![보안 그룹 세팅](/assets/img/posts/2024-01-08/elastic/set_security_group.png){: width="90%"}
`기본 세부 정보`에서 `보안 그룹 이름`과 `설명`을 입력하고 보안 그룹을 생성하고자 하는 `VPC`을 선택하고 위와 같이 `인바운드 규칙`을 추가한다. 하지만 인바운드 규칙에서 **0.0.0.0/0**은 최적의 방법은 아니다. 일단은 기초 실습 단계여서 **0.0.0.0/0**으로 설정하고 넘어가고자 한다.
7. `생성한 보안 그룹`을 앞의 단계에서 생성한 `Redis 캐시`와 연결해 준다.
  - 검색창에 `Elastic`를 입력하고, 검색 결과를 클릭한다.
  - 화면 좌측에 있는 탭에서 `Redis 캐시`를 클릭해서 생성된 `Redis 캐시 이름`을 클릭해 준다.
  ![Redis 캐시와 보안 그룹 연결](/assets/img/posts/2024-01-08/elastic/conn_redisCache_security.png){: width="90%"}
  - `보안` > `보안 그룹 선택됨` > `관리`에 들어가서 생성해 놓은 보안 그룹을 연결한다.

## 2. Elastic 연결
1. ElastiCache를 GUI로 볼 수 있게 하기 위해서 [redisinsight](https://redis.com/redis-enterprise/redis-insight/)에 들어가서 `redisinsight`을 다운받아 준다.
2. 외부에서 접근 가능한 경로를 통해 우회하는 방법을 사용하고자 `SSH Tunneling` 방법을 사용하고자 한다. 즉, 미리 만들어 놓은 `인스턴스`를 통해 SSH 터널링으로 `REDIS`에 접속할 통로를 만든다.
```ssh
ssh -i "key.pem 경로" -f -N -L 6379:REDIS_엔드포인트:6379 ubuntu@EC2_인스턴스_주소
```
3. 다운받아 놓은 `redisinsight`을 실행시켜 `Elastic`이 잘 연결되는지 확인한다.
![redisinsight](/assets/img/posts/2024-01-08/elastic/conn_redisinsight.png){: width="90%"}
아무런 설정을 바꾸지 않은 채 `Test Connection`을 눌러서 연결이 성공했는지 확인하고 redis database을 추가한다.
![redisinsight](/assets/img/posts/2024-01-08/elastic/conn_redisinsight_02.png){: width="90%"}
![redisinsight](/assets/img/posts/2024-01-08/elastic/conn_redisinsight_03.png){: width="90%"}
![redisinsight](/assets/img/posts/2024-01-08/elastic/conn_redisinsight_04.png){: width="90%"}
접속이 잘되는 것을 확인할 수 있다.

## 마치며
- 클라우드에 대한 대표적인 업체인 AWS을 활용해서 Elastic에 대한 기초적인 실습을 진행해 보았다.
- 익숙하지 않았던 만큼, 헤맨 시간도 많았지만 결국에는 연결에 성공해서 기분이 좋았다.
  - 초반에 개념이 정확하게 잡히지 않아서 어려웠던 시간이었던 것 같다. 클라우드에 있어서 중요한 개념인 만큼 추가적인 학습을 이어나갈 계획이다.
  - VPC, 보안 그룹 등 설정하는데 있어서 아직도 어렵지만 하나한 정복해 나간다는 생각으로 하다보니 뿌듯한 것 같기도 하다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**