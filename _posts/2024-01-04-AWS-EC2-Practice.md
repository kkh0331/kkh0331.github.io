---
title: AWS EC2 기초 실습
date: 2024-01-04 02:00:00 +0900
categories: [AWS 실습]
tags: [aws, EC2]
---
- `EC2`에 대한 개념, `EC2 생성`하고 `iTerm`을 활용해서 아나콘다와 flask를 설치하고 웹페이지에 접속하는 실습을 진행하고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.04` 기준이다.

## 1. EC2 개념
- EC2는 클라우드에서 안전하고 크기 조정이 가능한 컴퓨팅 용량을 제공하는 웹 서비스를 말한다.
- EC2는 `AMI`, `인스턴스 유형`, `네트워크 설정`, `스토리지 구성` 등 생성할 당시 설정해줘야 할 옵션이 많다.
- 하지만 온-프레미스 환경과 다르게 **몇 분 이내**로 서버를 생성할 수 있다는 점에서 굉장히 큰 장점으로 작용하는 것 같다.

## 2. EC2 생성
1. 사용자 계정으로 로그인하면 `콘솔 홈 화면`으로 들어와 진다.
- 루트 사용자의 안전을 보장하기 위해서 새로 만든 사용자 계정에서 실시하려고 한다.
2. 최근에 방문한 서비스에서 `EC2`을 선택하거나 검색창에 `EC2`을 입력하고, 검색 결과를 클릭한다.
![최근에 방문한 서비스에서 선택](/assets/img/posts/2024-01-04/select_EC2_01.png){: width="90%"}
   또는
![검색 창에 EC2 입력하고 클릭](/assets/img/posts/2024-01-04/select_EC2_02.png){: width="90%"}
3. EC2 대시보드에서 `리전`을 **서울**로 변경한다.
![리전 변경](/assets/img/posts/2024-01-04/change_region.png){: width="90%"}
4. EC2 대시보드 화면 좌측에 있는 탭에서 `인스턴스`를 클릭하면 인스턴스 관리 페이지로 이동한다.
![인스턴스 클릭](/assets/img/posts/2024-01-04/click_instance.png){: width="90%"}
5. 인스턴스 관리 페이지에서 신규 인스턴스를 생성하기 위해서 `인스턴스 시작` 버튼을 클릭한다.
![인스턴스 시작 버튼 클릭](/assets/img/posts/2024-01-04/click_start_instance_btn.png){: width="90%"}
6. `이름` 빈 칸에 인스턴스 이름을 입력한다.
![인스턴스 이름 입력](/assets/img/posts/2024-01-04/input_instance_name.png){: width="90%"}
7. `AMI`와 `아키텍처`를 선택한다. 여기에서는 `AMI`은 `Ubuntu Server 22.04 LTS(HVM), SSD Volume Type`를 선택했고 `아키텍처`는 `64비트(Arm)`을 선택했다.
![AMI와 아키텍처 선택](/assets/img/posts/2024-01-04/select_AMI_architecture.png){: width="90%"}
8. `인스턴스 유형`을 선택한다. 여기에서는 `t4g.small`을 선택했다.
![인스턴스 유형 선택](/assets/img/posts/2024-01-04/select_instance_type.png){: width="90%"}
9. `키 페어(로그인)`을 선택한다. **이미 만들어 놓은 키 페어**를 선택하거나 **새롭게 키 페어를 만들어서** 선택한다. 여기에서는 `새 키 페어 생성`을 통해 새로운 키 페어를 만들어서 선택하고자 한다.
![새 키 페어 생성 버튼 클릭](/assets/img/posts/2024-01-04/select_key_pair_01.png){: width="90%"}
`키 페어 이름`을 입력하고 `키 페어 유형`은 **RSA**, `프라이빗 키 파일 형식`은 **.pem**을 선택하고 `키 페어 생성` 버튼을 클릭한다.
![새 키 페어 생성 버튼 클릭](/assets/img/posts/2024-01-04/select_key_pair_02.png){: width="90%"}
`키 페어 이름.pem`이 자동으로 다운로드되는데, 추후에 `EC2 인스턴스 연결`을 할 때 필요하므로 잘 저장해둬야 한다.
10. `네트워크 설정`을 진행한다. 여기에서는 따로 설정해 주지 않고 넘어가고자 한다. 다음 실습 때 자세히 알아볼 예정이다.
    ![EC2 네트워크 설정](/assets/img/posts/2024-01-04/ec2_network_setting.png){: width="90%"}
11. `스토리지 구성`을 선택한다. 나머지는 기본 설정으로 두고 `인스턴스 시작` 버튼을 눌러서 인스턴스를 새롭게 생성한다.
![스토리지 구성 선택](/assets/img/posts/2024-01-04/select_storage_setting.png){: width="90%"}
12. `모든 인스턴스 보기` 버튼을 클릭하여 인스턴스 관리 페이지로 이동한다.
![모든 인스턴스 보기 버튼 클릭](/assets/img/posts/2024-01-04/click_all_instance_btn.png){: width="90%"}

## 3. EC2 인스턴스 연결
1. 연결을 원하는 EC2 인스턴스를 클릭하고 `연결` 버튼을 클릭한다. 단, `인스턴스 상태`가 **실행 중**일 때만 가능하다.
![연결 버튼을 클릭](/assets/img/posts/2024-01-04/click_connection_btn.png){: width="90%"}
2. `인스턴스에 연결`의 `SSH 클라이언트`에 나와있는대로 실시한다.
![연결 연결 페이지](/assets/img/posts/2024-01-04/show_instance_connection_page.png){: width="90%"}
  - SSH 클라이언트를 엽니다. Mac을 사용하고 있기 때문에 **iTerm**을 사용했다.
  - 키를 공개적으로 볼 수 없도록 하기 위해서 아래 명령어를 선택한다.
  ```ssh
  chmod 400 "인스턴스를 생성할 때 지정했던 키 페어가 내 컴퓨터에 저장되어 있는 경로"
  ```
  - 퍼블릭 DNS을 사용하여 인스턴스에 연결한다. → `SSH 클라이언트 > 예`의 명령어를 복사해서 `키 페어 경로`만 수정해 주면 된다.
  ```ssh
  ssh -i "키 페어 경로" ubuntu@<퍼블릭 DNS 경로>
  ```
  - `Are you sure you want to coninue connecting (yes/no/[fingerprint])?`라는 질문에 `yes`을 입력하고 ENTER을 누른다.
3. OS를 업데이트를 실시한다.
```ssh
sudo apt update
```
4. 업그레이드를 실시한다.
```ssh
sudo apt upgrade
```
- `Do you want to continue[Y/n]`라는 질문에 `Y`을 입력하고 ENTER을 누른다.
- 아래와 같은 화면이 나오면, ENTER을 누르면 된다.
![ssh 연결 01](/assets/img/posts/2024-01-04/ssh_connection.png){: width="80%"}

5. 재부팅을 실시한다.
```ssh
sudo reboot
```

## 4. 인스턴스에 아나콘다 설치
1. 아래 명령어를 통해 서버에 재접속한다.
```ssh
ssh -i "키 페어 경로" ubuntu@<퍼블릭 DNS 경로>
```
2. [아나콘다 다운로드](https://www.anaconda.com/download#downloads)에 들어가서 인스턴스를 생성할 때, 설정했던 `AMI`과 `아키텍처`에 따라서 해당 경로를 복사해서 `wget` 명령어를 활용해서 다운로드한다.
```ssh
wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-aarch64.sh
```
3. 아나콘다 설치를 진행한다.
```ssh
bash Anaconda3-2023.09-0-Linux-aarch64.sh
```
  - 나오는 질문에 맞춰서 `ENTER` 또는 `yes`을 입력해 주면 된다.
4. 아래 명령어를 통해 `(base)`라는 conda에서 생성된 기본 가상 환경으로 들어와 준다.
```ssh
source .bashrc
```

## 5. Flask 설치하고 실행시켜 본다.
1. `web`이라는 가상환경을 생성하고 `web` 안에 `python 3.11 버전`을 설치한다.
```ssh
conda create -n web python=3.11
```
2. 새로운 가상환경으로 들어가서 python이 실제로 설치되었는지 확인한다.
```ssh
conda activate web // web 가상환경 접속
which python // python 경로 확인
```
3. flask을 설치한다.
```ssh
pip install flask
```
4. `web`이라는 폴더를 만들고 `web` 폴더로 들어간다.
```ssh
mkdir web
cd web
```
5. 아래 명령어를 통해 `app.py` 파일을 생성과 동시에 열어준다.
```ssh
vi app.py
```
6. `i`을 입력해주면 입력 모드로 들어가서 [기본 애플리케이션 코드](https://flask-docs-kr.readthedocs.io/ko/latest/quickstart.html)를 복사 붙여넣기 해주는데, 아래 사항을 수정해준다.
```python
app.run() -> app.run(host='0.0.0.0')
```
그 이후에 `ESC`을 누르고 `:wq`을 입력하고 ENTER를 누른다.
7. 아래 명령어를 통해 flask을 실행시켜서 접속해 보면 접속이 안되는 것을 알 수 있다.
```ssh
python app.py
```
8. 위 에러는 2가지 문제로 일어난다.
- 첫 번째로는 **Private IP와 Public IP 차이**
  - `python app.py`을 통해 나온 IP 주소는 `http://Private_IP:5000`라는 것을 알수 있다. 이 Private IP를 통해 연결이 불가능하므로, Private IP을 Public IP로 변경해준다.
  - [2. EC2 생성 - 4까지](#2-ec2-생성)을 참고해서 `인스턴스 관리 페이지`로 들어간다.
  -  해당 인스턴스를 클릭해서 Pulic IP을 확인이 가능하다.
  ![Public IP 확인](/assets/img/posts/2024-01-04/confirm_public_ip.png)
- 두 번째로는 **보안 규칙**에 위배
  - 인스턴스 보안 규칙을 확인해 보면 `인바운드 규칙`에서 허용된 포트 범위만 `22`만인 것을 알 수 있다. `5000`을 추가해 준다.
  - [2. EC2 생성 - 4까지](#2-ec2-생성)을 참고해서 `인스턴스 관리 페이지`로 들어간다.
  - 아래 과정을 통해서 `인바운드 규칙` 추가가 가능하다.
  ![Public IP 확인](/assets/img/posts/2024-01-04/ec2_security_01.png)
  ![인바운드 규칙 편집](/assets/img/posts/2024-01-04/ec2_security_02.png)
  ![인바운드 규칙 생성](/assets/img/posts/2024-01-04/ec2_security_03.png)
9. 이 후에 `http://Public_IP:5000`을 통해 연결이 가능하다.
![연결 성공](/assets/img/posts/2024-01-04/connection_success.png)

## 마치며
- 클라우드에 대한 대표적인 업체인 AWS을 활용해서 EC2에 대한 기초적인 실습을 진행해 보았다.
- 아나콘다를 설치하는 도중에 예기치 못한 에러를 만나서 고생했었다. 처음에는 스토리지를 최소를 잡아 설치를 진행했었는데 그로 인해 아나콘다를 설치할 용량이 부족해서 생기는 오류였다. 이를 해결하기 위해서 **인스턴스의 스토리지 용량을 변경하는 과정**을 거치곤 했다.
- Private IP와 Public IP 사이의 차이를 이해하는 것이 필요할 것 같다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**
  