---
title: AWS RDS 기초 실습
date: 2024-01-08 02:00:00 +0900
categories: [AWS 실습]
tags: [aws, RDS]
---
- `RDS 생성`하고 `DataGrip`과 `기존에 생성해 둔 jupyter을 가진 EC2`을 활용해서 `RDS`에 테이블을 생성하고 데이터를 추가해보면 실습을 진행하고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.05` 기준이다.

## 1. RDS 생성
1. 사용자 계정으로 로그인하면 `콘솔 홈 화면`으로 들어와 진다. 
- 루트 사용자의 안전을 보장하기 위해서 새로 만든 사용자 계정에서 실시하려고 한다.
2. 먼저, [VPC 실습](/posts/AWS-VPC-Practice/) 참고하여 `VPC` 안에 2개의 가용영역을 할당했고 각 가용영역 안에 서브넷을 만들었다. [EC2 실습](/posts/AWS-EC2-Practice/)을 참고하여 서브넷 중 하나에 jupyter을 가진 서브넷을 만들었다.
3. `데이터베이스` 생성하기 위해서 필요한 사전 설정을 하고자 한다.
- 생성할 때, 발생할 수 있는 `DNS` 관련 에러를 막기 위해서 검색창에 `VPC`를 입력하고, 검색 결과를 클릭하고 아래와 같은 설정을 하였다.
![RDS 생성을 위한 VPC 사전 설정](/assets/img/posts/2024-01-08/rds/set_vpc_to_make_rds.png)
  - `DNS 설정`에서 `DNS 확인 활성화`와 `DNS 호스트 이름 활성화` 둘 다 체크하고 `저장` 버튼을 클릭한다. 
- `DB 서브넷 그룹`을 설정한다. `DB 서브넷 그룹`은 생성하고자 하는 `RDS`을 사용할 수 있는 `가용 영역들`을 사전에 그룹화 해준다.
![RDS 생성을 위한 DB 서브넷 그룹 사전 설정](/assets/img/posts/2024-01-08/rds/set_subgroup_to_make_rds.png)
`이름`, `설명`, RDS을 성성하고자 하는 `VPC`을 선택하고 RDS을 사용할 수 있는 `가용 영역`과 `서브넷`을 선택한다. 여기에서는 모든 `가용 영역`과 `서브넷`을 선택했다.
![RDS 생성을 위한 DB 서브넷 그룹 사전 설정](/assets/img/posts/2024-01-08/rds/set_subgroup_to_make_rds_02.png)
4. 화면 좌측에 있는 탭에서 `데이터베이스`를 클릭해서 데이터베이스 관리 페이지로 이동하여 `데이터베이스 생성` 버튼을 클릭한다.
![데이터베이스 생성 버튼 클릭](/assets/img/posts/2024-01-08/rds/click_create_db_btn.png){: width="90%"}
5. 아래와 같이 세팅한다.
![데이터베이스 세팅 01](/assets/img/posts/2024-01-08/rds/set_rds.png){: width="90%"}
![데이터베이스 세팅 02](/assets/img/posts/2024-01-08/rds/set_rds_02.png){: width="90%"}
`엔진 버전`은 기본값으로 설정한다.
![데이터베이스 세팅 03](/assets/img/posts/2024-01-08/rds/set_rds_03.png){: width="90%"}
`인스턴스 구성`과 `스토리지`는 기본 설정되어 있는 그대로 설정했다.
![데이터베이스 세팅 04](/assets/img/posts/2024-01-08/rds/set_rds_04.png){: width="90%"}
![데이터베이스 세팅 05](/assets/img/posts/2024-01-08/rds/set_rds_05.png){: width="90%"}
![데이터베이스 세팅 06](/assets/img/posts/2024-01-08/rds/set_rds_06.png){: width="90%"}
나머지는 기본 세팅으로 설정하고 RDS을 생성한다.
6. 생성된 `DB 식별자`를 클릭하여 `연결 및 보안` > `VPC 보안 그룹`을 선택하여 `Security group ID`을 선택해서 `인바운드 규칙 편집`을 클릭한다.
![보안 그룹 세팅](/assets/img/posts/2024-01-08/rds/set_security_group.png){: width="90%"}
위와 같은 사항을 추가한다. 이것은 Public한 설정이나 선호되는 방법은 아니지만 일단은 여기에서는 이렇게 진행했다.
7. 추가적으로 `VPC`의 `서브넷`에는 `인테넷 게이트웨이`가 `라우팅 테이블`을 통해 연결되어 있어야 한다.([AWS VPC 기초 실습](/posts/AWS-VPC-Practice/) 참고)

## 2. RDS 연결
1. `DataGrip`을 열어서 `RDS`와 연결해준다.
![DataGrip과 RDS 연결](/assets/img/posts/2024-01-08/rds/conn_RDS_DataGrip.png){: width="90%"}
![DataGrip과 RDS 연결](/assets/img/posts/2024-01-08/rds/conn_RDS_DataGrip_02.png){: width="90%"}
2. [AWS VPC 기초 실습](/posts/AWS-VPC-Practice/)을 참고하여 juptyter를 실행시켜 준다.
3. jupyter에 접속하여 코드를 작성한다. 코드는 간단한 SQL문으로 아래와 같다.

```python
import pymysql # sql에 접속하기 위해서

def dbconnect():
  conn = pymysql.connect(
    host = 'RDS의 엔드포인트', # RDS의 '연결 및 보안'에서 확인 가능하다.
    user = 'RDS 생성할 때 작성했었음', # RDS의 '구성' > '마스터 사용자 이름'에서 확인 가능하다. 
    password = 'RDS 생성할 때 작성했었음', # 확인 불가 -> 본인이 기억하고 있어야 한다.
    db = '최초의 데이터베이스', # RDS의 '구성' > 'DB 이름'에서 확인 
    charset = 'utf8',
    port = 3306
  )
  return conn

def conn_db(sql):
  conn = dbconnect()
  cur = conn.cursor()
  cur.execute(sql)
  conn.commit()
  conn.close()

if __name__ == '__main__':
  sql = "CREATE TABLE test (id INT AUTO_INCREMENT PRIMARY KEY, num INT, data VARCHAR(255));"
  conn_db(sql) # 3.1. test라는 테이블 생성

  sql = "INSERT INTO test (num, data) VALUES (100, 'data01');"
  conn_db(sql) # 3.2. test라는 테이블에 데이터 삽입
```

- 3.1. test라는 테이블 생성이 되면 아래와 같다.
![test라는 테이블 생성](/assets/img/posts/2024-01-08/rds/create_db.png){: width="90%"}
- 3.2. test라는 테이블에 데이터 삽입이 되면 아래와 같다.
![test라는 테이블에 데이터 삽입](/assets/img/posts/2024-01-08/rds/insert_data.png){: width="90%"}

## 마치며
- 클라우드에 대한 대표적인 업체인 AWS을 활용해서 RDS에 대한 기초적인 실습을 진행해 보았다.
- Public으로 연결하는 방법이 Best Practice는 아닌 만큼, Private로 연결을 해보는 연습도 필요할 것 같다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**