---
title: AWS VPC 기초 실습
date: 2024-01-05 01:00:00 +0900
categories: [AWS 실습]
tags: [aws, VPC, jupyter]
---
- `VPC 생성`하고 `iTerm`을 활용해서 아나콘다와 jupyter를 설치하고 실행까지 해보는 실습을 진행하고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.05` 기준이다.

## 1. Public 연결을 위한 VPC 구성
- `리전`을 항상 **서울**로 유지한다.
- `tag`는 본인이 원한는대로 설정한다.
- VPC와 서브넷의 `CIDR 블록`은 보통 사용되는 주소를 선택한다.

### 1.1. VPC 생성
1. 사용자 계정으로 로그인하면 `콘솔 홈 화면`으로 들어와 진다.
- 루트 사용자의 안전을 보장하기 위해서 새로 만든 사용자 계정에서 실시하려고 한다.
2. 검색창에 `VPC`를 입력하고, 검색 결과를 클릭한다.
![검색 창에 EC2 입력하고 클릭](/assets/img/posts/2024-01-05/select_VPC.png){: width="90%"}
3. 화면 좌측에 있는 탭에서 `VPC`를 클릭해서 VPC 관리 페이지로 이동하여 `VPC 생성` 버튼을 클릭한다.
![VPC 생성 버튼 클릭](/assets/img/posts/2024-01-05/click_create_VPC_btn.png){: width="90%"}
4. VPC 관련된 내용들을 아래와 같이 세팅하고 `VPC 생성` 버튼을 클릭해 VPC을 생성한다.
![VPC 세팅](/assets/img/posts/2024-01-05/set_VPC.png){: width="90%"}

### 1.2. 서브넷 생성
1. 화면 좌측에 있는 탭에서 `서브넷`을 클릭해서 서브넷 관리 페이지로 이동하여 `서브넷 생성` 버튼을 클릭한다.
![서브넷 생성 버튼 클릭](/assets/img/posts/2024-01-05/click_create_subnet_btn.png){: width="90%"}
2. 서브넷 관련된 내용들을 아래와 같이 세팅하고 `서브넷 생성` 버튼을 클릭해 서브넷을 생성한다.
![VPC 세팅](/assets/img/posts/2024-01-05/set_subnet.png){: width="90%"}

### 1.3. 인터넷 게이트웨이 생성
1. 화면 좌측에 있는 탭에서 `인터넷 게이트웨이`를 클릭해서 인터넷 게이트웨이 관리 페이지로 이동하여 `인터넷 게이트웨이 생성` 버튼을 클릭한다.
![인터넷 게이트웨이 생성 버튼 클릭](/assets/img/posts/2024-01-05/click_internet_gateway_btn.png){: width="90%"}
2. 인터넷 게이트웨이 이름을 입력하고 `인터넷 게이트웨이 생성` 버튼을 클릭해 인터넷 게이트웨이 생성한다.
![인터넷 게이트웨이 세팅](/assets/img/posts/2024-01-05/set_internet_gateway.png){: width="90%"}
3. 인터넷 게이트웨이와 VPC을 연결해준다.
![인터넷 게이트웨이와 VPC 연결](/assets/img/posts/2024-01-05/connect_VPC_internet_gateway.png){: width="90%"}
![인터넷 게이트웨이와 VPC 연결](/assets/img/posts/2024-01-05/connect_VPC_internet_gateway_02.png){: width="90%"}

### 1.4. 라우팅 테이블 생성
1. 화면 좌측에 있는 탭에서 `라우팅 테이블`을 클릭해서 라우팅 테이블 관리 페이지로 이동하여 `라우팅 테이블 생성` 버튼을 클릭한다.
![인터넷 게이트웨이 생성 버튼 클릭](/assets/img/posts/2024-01-05/click_routing_table_btn.png){: width="90%"}
2. 라우팅 테이블 관련된 내용들을 아래와 같이 세팅하고 `라우팅 테이블 생성` 버튼을 클릭해 라우팅 테이블을 생성한다.
![라우팅 테이블 세팅](/assets/img/posts/2024-01-05/set_routing_table.png){: width="90%"}
3. 라우팅 테이블 관리 페이지에서 `생성된 라우팅 테이블 ID`을 클릭해서 해당 라우팅 테이블 관리 페이지로 들어온다.
4. 필요한 경우, 라우팅 테이블과 `전에 생성해 둔 인터넷 게이트웨이`를 연결해준다. 여기에서는 외부와 통신하기 위해서 설정해 주려고 한다.
![인터넷 게이트웨이와 라우팅 테이블 연결](/assets/img/posts/2024-01-05/connect_internet_gateway_routing_table.png){: width="90%"}
![인터넷 게이트웨이와 라우팅 테이블 연결](/assets/img/posts/2024-01-05/connect_internet_gateway_routing_table_02.png){: width="90%"}
5. 아래와 같이 `서브넷 연결 편집`으로 들어가서 라우팅 테이블과 연결을 원하는 서브넷과 연결한다.
![서브넷과 라우팅 테이블 연결](/assets/img/posts/2024-01-05/connect_subnet_routing_table.png){: width="90%"}

### 1.5. 보안 그룹 생성
1. 화면 좌측에 있는 탭에서 `보안 그룹`을 클릭해서 보안 그룹 관리 페이지로 이동하여 `보안 그룹 생성` 버튼을 클릭한다.
![보안 그룹 생성 버튼 클릭](/assets/img/posts/2024-01-05/click_security_group_btn.png){: width="90%"}
2. 보안 그룹을 아래와 같이 세팅한다. 여기에서는 외부와 통신을 원하기 때문에 아래와 같이 세팅한다.
![보안 그룹 세팅](/assets/img/posts/2024-01-05/set_security_group.png){: width="90%"}
![보안 그룹 세팅](/assets/img/posts/2024-01-05/set_security_group_02.png){: width="90%"}
![보안 그룹 세팅](/assets/img/posts/2024-01-05/set_security_group_03.png){: width="90%"}

### 1.6. EC2 생성
1. [AWS EC2 기초 실습](/posts/AWS-EC2-Practice/)의 `2. EC2 생성`을 참고하되 jupyter을 생성하기 위해서 아래와 같은 사항을 수정한다.
- `아키텍처` : `64비트(Arm)` -> `64비트(x86)`
- `인스턴스 유형` : `t4g.small` -> `m5.xlarge`
- `스토리지 구성` : `16 GiB` -> `20 GiB`
- `네트워크 수정`은 아래와 같다.
![EC2 네트워크 설정](/assets/img/posts/2024-01-05/set_ec2.png)


## 2. jupyter 설치 및 실행
1. 위에서 생성한 EC2를 가지고 [AWS EC2 기초 실습](/posts/AWS-EC2-Practice/)의 `3. EC2 인스턴스 연결`과 `4. 인스턴스에 아나콘다 설치`을 참고하여 `(base) 가상환경`으로 들어와 준다.
2. 아래 명령어를 통해 **jupyter**을 설치하고 그 **jupyter** 안에 파이썬을 설치해 준다.
```ssh
conda create -n jupyter python=3.11
```
3. jupyter 환경으로 들어와 준다.
```ssh
conda activate jupyter
```
4. jupyterlab을 설치해 준다.
```ssh
pip install jupyterlab
```
5. jupyter을 실행해 준다.
```ssh
jupyter lab --ip=0.0.0.0
```
6. [AWS EC2 기초 실습](/posts/AWS-EC2-Practice/)의 `5. Flask 설치하고 실행시켜 본다.`의 `8. 위 에러는 ~ `와 같은 에러가 발생한다.
- Private IP 대신에 Public IP로 접속한다.
- `인바운드 규칙`에 포트 번호 `8888`을 추가해준다.
7. 이후에는 iTerm에서 나온 주소에서 Public IP로만 수정해 주면 실행이 된다.
![jupyter 실행](/assets/img/posts/2024-01-05/start_jupyter.png){: width="90%"}

## 마치며
- 클라우드에 대한 대표적인 업체인 AWS을 활용해서 VPC에 대한 기초적인 실습을 진행해 보았다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**