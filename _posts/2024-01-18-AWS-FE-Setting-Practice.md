---
title: AWS 프론트엔드 셋팅하기
date: 2024-01-18 00:00:00 +0900
categories: [AWS 실습]
tags: [aws, s3, cloudfront]
--- 
- 이번 포스터에서는 [CSS 기초](/posts/CSS-Basis/)에서 진행했던 `프로필 페이지` 실습 내용을 AWS로 배포하는 실습을 진행하려고 한다.
- AWS에서는 S3와 CloundFront을 이용하려고 한다.

## 1. S3란 무엇인가?
- `S3`는 `Simple Storage Service`의 약자
- AWS가 제공하는 **클라우드 스토리지 서비스**로 특정한 사진, 동영상 등의 파일을 저장하기 위해 사용할 수 있느 서비스
- 저장하는 데이터 양에 대한 비용도 저렴하고, 저장할 수 있는 데이터 양도 무한에 가까움
- `S3`을 사용하기 위해서는 `버킷`을 생성해야 되고, 이 `버킷`의 이름은 전 세계에서 유일

## 2. CloudFront란 무엇인가?
- AWS의 `CDN(Content Delivery Netwokr)` 서비스
- CDN 서비스 : Client의 content 요청으로 서버에서 받아온 content를 **캐싱하고 이후 같은 요청이 왔을 때, 그 캐싱해 둔 것을 제공하는 서비스**
- 이로 인해, 물리적으로 거리가 먼 곳에도 빠르게 요청을 처리할 수 있고 결과적으로 서버의 부하를 낮출 수 있음

## 3. 배포 실습
- 진행했던 프로젝트를 배포하기 위해서 아래와 같은 순서로 진행됨
  - 1. `S3의 버킷`을 생성하여 저장소를 만들어 줌.
  - 2. `IAM`으로 권한을 설정 → `액세스 키 ID`와 `비밀 액세스 키` 발급
  - 3. `Github`와 `S3의 버킷`을 연결해줌.
  - 4. `CloudFront`와 `S3의 버킷`을 연결함으로써 배포를 진행함.

### 3.1. `S3의 버킷` 생성
1. AWS에 로그인하여 `S3`라고 검색 한 뒤에, `버킷 만들기` 버튼을 클릭하여 `버킷`을 생성해준다.
![S3 생성 01](/assets/img/posts/2024-01-18/create_s3_bucket_01.png){:width="90%"}
`AWS 리전`을 선택하고 원하는 `버킷 이름`을 입력한다.
![S3 생성 02](/assets/img/posts/2024-01-18/create_s3_bucket_02.png){:width="90%"}
나머지는 기본값으로 두고 `버킷 만들기` 버튼을 클릭한다.
2. 생성된 버킷 관리 페이지에서 `속성` > `정적 웹사이트 호스팅` > `편집`에서 아래와 같이 세팅한다.
![정적 웹사이트 포스팅 서정](/assets/img/posts/2024-01-18/static_web_site_hosting_setting.png){:width="90%"}
3. 생성된 버킷 관리 페이지에서 `권한` > `버킷 정책`의 `편집`에서 아래 코드를 추가한다.
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::'버킷 이름'/*"
            }
        ]
    }
    ```

### 3.2. IAM 권한 설정
1. [AWS IAM 기초 실습](/posts/AWS-IAM-Practice/)을 참고하여 `IAM`을 생성해 준다.
- 여기에서는 Github을 연결하기 위해 필요한 키 발급이 목적이므로 `AWS Management Console에 대한 사용자 엑세스 권한 제공`을 체크하지 않고 넘어간다.
- `직접 정책 연결`에서 `AmazonS3FullAccess`와 `CloudFrontFullAccess`에 대한 정책을 선택한다.
  - `AmazonS3FullAccess` : 프로젝트에 대한 변경사항을 `Github`에 push하면 그 내용이 `S3`에 그대로 반영하기 위해 필요하다. → CI/CD(지속적 통합/지속적 배포)
  - `CloudFrontFullAccess` : `Github`에 push하여 `S3`에서는 업데이트가 된다. 하지만 `CloudFront(캐시 서버)`에서는 반영이 되지 않을 수 있기 때문에 `Github`에 `CloudFront` 무효화 기능을 주기 위해 필요하다.
2. 생성된 IAM 관리 페이지에서 `보안 자격 증명` > `액세스 키` > `액세스 키 만들기` > `기타`를 선택하여 `액세스 키 ID`와 `비밀 액세스 키` 발급한다.

### 3.3. Github와 연결해줌.
1. 진행하던 프로젝트의 `Github`에 접속하여 `Actions` > `set up a workflow yourself`을 클릭한다.
![workflow 생성](/assets/img/posts/2024-01-18/set_up_workflow.png){:width="90%"}
`프로젝트 이름`/`.github`/`workflows`/`main.yml`에 아래와 같은 코드를 추가한다.

    ```yml
    name: Deploy to S3

    on:
    push:
        branches:
        - master # 배포할 branch

    jobs:
    deploy:
        runs-on: ubuntu-latest

        steps:
        - name: Checkout Repository
            uses: actions/checkout@v2

        - name: Configure AWS Credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            aws-region: ap-northeast-2 # AWS S3 버킷이 위치한 리전으로 변경

        - name: Upload to S3
            run: | # 밑에서 web은 github에서 web 폴더를 의미
            aws s3 sync web s3://{내_S3_버킷_이름}/
    ```

2. 진행하던 프로젝트의 `Github`의 `Settings` > `Security` > `Secrets and variables` > `Actions`의 `Repository secrets`에서 아까 발급해 둔 `액세스 키 ID`와 `비밀 액세스 키`를 입력한다.
![액세스 키 github에 연결](/assets/img/posts/2024-01-18/input_keys_in_github.png){:width="90%"}
3. 그런 다음, `AWS` > `S3` > `생성해둔 버킷 관리 페이지`에서 Github에 있는 `web`에 있는 내용들이 들어있는지 확인하다.
![연결된 객체들 확인](/assets/img/posts/2024-01-18/confirm_objects.png){:width="90%"}

### 3.4. CloudFront을 생성하고 배포 진행
1. AWS에서 `CloudFront`라고 검색 한 뒤에, `배포 생성` 버튼을 클릭하여 배포를 진행해준다.
2. 아래 사항만 제외하고는 기본값으로 설정해준다.
- `원본 도메인`에는 `생성해둔 S3버킷` > `속성` > `정적 웹사이트 호스팅` > `url`에서 `https://`을 제외하고 입력해준다.
- `이름`을 알아보기 쉽게 설정한다.
- `기본 캐시 동작` > `뷰어 프로토콜 정책` > `Redirect HTTP to HTTPS`을 선택해준다.
3. `도메인 이름`에 접속하여 실제로 배포가 진행이 되었는지 확인한다.
4.  `Github`에 `CloudFront` 무효화 기능을 추가해 주기 위해서 `프로젝트 이름`/`.github`/`workflows`/`main.yml`의 `steps:`에 아래와 같은 코드를 추가해준다.
```yml
- name: Invalidate cloudfront
  run: aws cloudfront create-invalidation --distribution-id 'CloudFront ID' --paths /*
```

## 4. 결과
- [Github 주소](https://github.com/kkh0331/html-css)
- 배포 주소 <https://dex2le68agvf4.cloudfront.net/>

## 마치며
- 나중에 혹시라도 정적인 페이지를 배포할 일이 있다면 굉장히 유용한 방법이 될 것 같다.
- 나중에 프로젝트를 진행할 때, 여러 가지 AWS 요소를 사용할 것이라고 예상되는 만큼 AWS에 대한 더 깊은 공부가 필요할 것 같다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**