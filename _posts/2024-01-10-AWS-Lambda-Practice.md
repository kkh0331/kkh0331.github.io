---
title: AWS Lambda 기초 실습
date: 2024-01-10 00:00:00 +0900
categories: [AWS 실습]
tags: [aws, Lambda, ChatGPT]
---
- `Lambda`에 대한 기초 실습과 더 나아가 ChatGPT와 연결해 보는 실습을 진행해 보고자 한다.
- 참고로, **AWS의 홈페이지는 자주 바뀌기 때문에** 이 포스터의 실습 관련된 이미지들은 `2024.01.10` 기준이다.

## 1. 간단한 AWS Lambda 만들기
### 1.1. AWS Lambda 함수 생성
1. AWS의 `콘솔 홈 화면`에서 `Lambda`을 입력하고, 검색 결과를 클릭한다.
2. 화면 좌측에 있는 탭에서 `함수`를 클릭하고 `함수 생성` 버튼을 클릭한다.
![함수 생성 버튼 클릭](/assets/img/posts/2024-01-10/lambda/click_create_function_btn.png){: width="90%"}
3. `함수`와 관련된 내용 세팅한다.
![함수 내용 세팅](/assets/img/posts/2024-01-10/lambda/set_function_01.png){: width="90%"}
![함수 내용 세팅](/assets/img/posts/2024-01-10/lambda/set_function_02.png){: width="90%"}
4. 스크롤을 내려서 `코드 소스`에서 `Test` 버튼을 클릭한다.
![함수 테스트 생성](/assets/img/posts/2024-01-10/lambda/create_test_function.png){: width="90%"}
![함수 테스트 생성](/assets/img/posts/2024-01-10/lambda/create_test_function_02.png){: width="90%"}
5. `Test` 버튼을 클릭하면, `Execution results`을 통해 확인 가능하다. 만약 수정할 경우, `Test` 버튼 옆의 `Deploy` 버튼을 클릭 후에 `Test` 버튼을 클릭한다.

### 1.2. 트리거로 API Gateway 연결
1. 트리거를 통해 `API Gateway`와 연결한다.
![트리거 추가](/assets/img/posts/2024-01-10/lambda/add_trigger.png){: width="90%"}
- `API Gateway`을 클릭 
- `의도` > `새 API 생성`을 선택
- `API 유형` > `REST API`을 선택
- `보안` > `열기`
- `추가` 버튼을 클릭하다.

### 1.3. API Gateway 배포하여 Endpoint 생성
1. `API Gateway`을 클릭하여 API Gateway 관리 페이지로 들어간다.
![API Gateway 관리 페이지로 들어감](/assets/img/posts/2024-01-10/lambda/deploy_api_gateway.png){: width="90%"}
2. `API Gateway`을 배포한다.
![API 배포](/assets/img/posts/2024-01-10/lambda/deploy_api_gateway_02.png){: width="90%"}
원하는 `스테이지`를 클릭해서 `배포` 버튼을 클릭한다.
3. 배포되었는지 확인한다.
- API 관리 페이지 화면 좌측에 있는 탭에서 `스테이지`를 클릭해서 `스테이지 세부 정보` > `URL` 확인한다.
![API 배포 확인](/assets/img/posts/2024-01-10/lambda/confirm_api_deployment.png){: width="90%"}
-  API 관리 페이지 화면 좌측에 있는 탭에서 `리소스`에서 추가 경로 확인 가능하다
![API 배포 확인](/assets/img/posts/2024-01-10/lambda/confirm_api_deployment_02.png){: width="90%"}
- 주소창에 입력해서 배포되었는지 확인
![API 배포 확인](/assets/img/posts/2024-01-10/lambda/confirm_api_deployment_03.png){: width="90%"}

## 2. ChatGPT 연결
1. ChatGPT의 `API Key`를 개인적으로 발급받는다.
2. [CoreDotToday의 lambda-openai-test](https://github.com/CoreDotToday/lambda-openai-test)에서 `zip` 형식으로 다운로드 받아준다.
3. 아래 그림에서 `에서 업록드`을 클릭해서 다운받은 `zip`을 업로드한다.
![API 배포 확인](/assets/img/posts/2024-01-10/lambda/upload_download_zip.png){: width="90%"}
4. `test-01` 밑의 모든 파일이 있도록 경로를 수정해준다.(드래그 앤 드롭 이용)
![경로 수정](/assets/img/posts/2024-01-10/lambda/fix_route.png){: width="90%"}
5. lambda_function.py을 아래 내용으로 수정해 준다.

```python
import openai

openai.api_key = '개인적으로 발급받은 API Key' 

def lambda_handler(event, context):
  message = openai.ChatCompletion.create(
    model="gpt-3.5-turbo", #사용모델
    messages=[
      {"role": "system", "content": "너는 클라우드 전문 개발자야."},
      {"role": "user", "content": "나는 신한투자증권에 입사하려는 개발자야. 클라우드를 공부해야 할 필요가 있을까?"}
    ] #전달메세지 
  )
  return message
```
- `Deploy` 버튼을 클릭하고 `Test`을 진행한다.
- `Time out` 관련 에러 날 경우, `구성` > `일반 구성` > `편집`을 눌러 `제한 시간`을 늘려준다.
- 다시 한 번 `Test` 버튼을 클릭해서 결과를 확인해 본다.
![chatgpt 결과](/assets/img/posts/2024-01-10/lambda/chatgpt_result.png){: width="90%"}

## 3. [1.3](#13-api-gateway-배포하여-endpoint-생성)에서 배포된 url에서 작동되도록 수정
1. `API Gateway`을 클릭하여 API Gateway 관리 페이지로 들어간다.
![API Gateway 관리 페이지로 들어감](/assets/img/posts/2024-01-10/lambda/deploy_api_gateway.png){: width="90%"}
2. `통합 요청` > `편집`으로 들어가서 `Lambda 프록시 통합`을 false로 수정하고 `API 배포`를 클릭한다.
![chatgpt api 배포](/assets/img/posts/2024-01-10/lambda/deploy_chatgpt_api.png){: width="90%"}
3. [1.3](#13-api-gateway-배포하여-endpoint-생성)에서 확인했던 경로로 확인해 본다.
![chatgpt api 확인](/assets/img/posts/2024-01-10/lambda/confirm_chatgpt_api.png){: width="90%"}

## 4. API Gateway에 파라미터 추가
1. `API Gateway`을 클릭하여 API Gateway 관리 페이지로 들어간다.
![API Gateway 관리 페이지로 들어감](/assets/img/posts/2024-01-10/lambda/deploy_api_gateway.png){: width="90%"}
2. `통합 요청` > `편집`을 클릭해서 `요청 본문 패스스루` > `정의된 템플릿이 없는 경우(권장)`을 선택하고 `매핑 템플릿` > `매핑 템플릿 추가` 클릭한다. 
![매핑 템플릿 추가](/assets/img/posts/2024-01-10/lambda/add_mapping_template.png){: width="90%"}
`저장`을 누르고 `API 배포`를 진행한다.
3. 함수 test-01 관리 페이지로 돌아가서 `lambda_function.py`을 아래처럼 수정하고 `Deploy`한다.

```python
import openai

openai.api_key = '개인적으로 발급받은 API Key' 

def lambda_handler(event, context):
  if 'm' in event['params']['querystring']:
    input_message = event['params']['querystring']['m']
  else:
    input_message = "나는 신한투자증권에 입사하려는 개발자야. 클라우드를 공부해야 할 필요가 있을까?"

  message = openai.ChatCompletion.create(
    model="gpt-3.5-turbo", #사용모델
    messages=[
      {"role": "system", "content": "너는 클라우드 전문 개발자야."},
      {"role": "user", "content": input_message}
    ] #전달메세지 
  )
  return message
```
- [1.3](#13-api-gateway-배포하여-endpoint-생성)에서 확인했던 경로에서 `?m=<질문>`을 추가로 입력해준다. 결과는 아래와 같다.
![chatgpt 파라미터 결과](/assets/img/posts/2024-01-10/lambda/result_chatgpt_query.png)

## 마치며
- Lambda을 이용한 api 배포 및 ChatGPT을 활용한 실습까지 해보았다.
- Lambda라는 개념이 아직은 익숙하지 않아서 추가적으로 공부할 계획이다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**