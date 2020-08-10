---
title: "Rest APIs for CI/CD"
linkTitle: "Rest APIs"
weight: 4
description: >

#   What does your user need to understand about your project in order to use it - or potentially contribute to it?
---

## Integrating REST APIs into CI / CD pipelines

CI (Continuous Integration) 도구과 연동하여 apptest.ai를 사용하기위한 API 정보를 제공합니다. 해당 문서에서 제공되는 테스트시작, 테스트 상태 보기, 테스트 결과 조회 API를 통해 apptest.ai를 사용하실 수 있습니다.

### 1. [POST] Run New Testset

새로운 테스트 세트를 생성합니다. (테스트 세트는 단위 테스트들의 집합니다.)<br/>
새로운 테스트기 실행될때 사용되는 일부 옵션들은 프로젝트에 미리 저장해놓은 설정 정보를 따릅니다.

[Request]

```
POST /openapi/v2/testset

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request Body Miltipary Form Data #1

| key      | type | Description           | Required |
| :------- | :--- | :-------------------- | :------- |
| app_file | File | 테스트할 대상 앱 파일 | Required |

Request Body Miltipary Form Data #2

| key          |          | type            |                       | Description                                                                                                                   | Required |
| :----------- | :------- | :-------------- | :-------------------- | :---------------------------------------------------------------------------------------------------------------------------- | :------- |
| pid          |          | Positive Number | -                     | 프로젝트의 고유번호 <br/> ex) 509                                                                                             | Required |
| testset_name |          | String          | Max 100 Characters    | 테스트 세트의 이름 <br/> ex) "Testset name Example#1"                                                                         | Required |
| time_limmit  |          | Positive Number | Min : 5 <br/>Max : 30 | 테스트 제한 시간 (Minutes) <br/>ex) 5 <br/>값이 비어 있으면 프로젝트 설정에 저장된 시간 제한값을 따릅니다.                    | Required |
| use_vo       |          | Boolean         | Default: false        | AT&T Video Optimizer(ARO) 사용 여부 (`true` or `false` ) <br/>ex) true                                                        | Required |
| callback     |          | String          | Max 250 Characters    | 테스트가 완료될 경우 호출될 Callback URL <br/>ex) 'https://127.0.0.1/callback/url/example'                                    | Required |
| credentials  | login_id | String          | Max 150 Characters    | 앱을 테스트하는데 사용될 테스트 대상앱의 계정정보(아이디) <br/>(Test credentials info - Login ID) <br/>ex) 'credentials_id'   | Required |
|              | login_pw | String          | Max 150 Characters    | 앱을 테스트하는데 사용될 테스트 대상앱의 계정정보(비밀번호) <br/>(Test credentials info - Login PW) <br/>ex) 'credentials_pw' | Required |

[ Request Example ]

```
curl    --request POST \
        --user {user_id}:{access_key} \
        -F 'app_file=@/path/of/your/app_file' \
        -F 'data={\
            "pid": 509, \
            "testset_name": "Testset Name Example #1", \
            "time_limit": 5, \
            "use_vo": false, \
            "credentials": { \
                "login_id": "credentials_id", \
                "login_pw": "credentials_pw" \
            } \
        }' https://api.apptest.ai/openapi/v2/testset
```

[ Response ]

| key        |     | type            | Description                                         |
| :--------- | :-- | :-------------- | :-------------------------------------------------- |
| test_count |     | Positive Number | 테스트 세트에 실행된 단일 테스트의 갯수 <br/> ex) 3 |
| testset_id |     | Positive Number | 테스트 세트의 고유번호 <br/> ex) 251929             |

[ Response Example ]

```
{
    "data": {
        "test_count": 1,
        "testset_id": 251929
    },
    "result_code": 0,
    "result_msg": "",
    "reason": ""
}
```

[ Error Code ]

| result_code | result_msg                | reason                                                                                                                                                                 |
| :---------- | :------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 4000        | Missing Request Parameter | The required parameter was not found in the request parameters : {{ PARAMETER KEY }}                                                                                   |
| 4030        | App File Analysis Failed  | iOS plistlib Parsing Error : {{ PARSING ERROR MSG }}                                                                                                                   |
| 4031        |                           | Test creation failed : There was a problem pre-processing your app file. \n The IPA file must be a development version that is signed by your development certificate. |
| 4032        |                           | Test creation failed. Invalid app file. \n There was a problem pre-processing your app file.                                                                           |
| 4040        |                           | Android Manifest Parsing Error : {{ PARSING ERROR MSG }}                                                                                                               |
| 4041        |                           | No launchable activity found. (android.intent.category.LAUNCHER)                                                                                                       |
| 4050        | Invalid Request Parameter | File extension not supported. ( Support : ipa, zip, app, apk, xapk, apks )                                                                                             |
| 4051        |                           | The required parameter was not found in the request parameters : (app_file)                                                                                            |
| 4052        |                           | Invalid request parameter : (project_id: {{ PROJECT_ID }})                                                                                                             |
| 4053        |                           | Invalid request parameter : (testset_id: {{ TESTSET_ID }})                                                                                                             |
| 4055        |                           | Device is not compatible with this app : ( APP os type : {{ APP_OS_TYPE }} )                                                                                           |
| 4080        | Invalid Request Parameter | Device info does not exist in prese project data.                                                                                                                      |
| 4081        |                           | Available devices does not exist in preset project data.                                                                                                               |
| 5001        | Run New Testset Error     | Global Exception Error - {{ ERROR_MSG }}                                                                                                                               |
| 6001        | App File Upload Error     | App file upload fail.                                                                                                                                                  |
| 8000        | System Maintenance        | The service is currently under system maintenance.                                                                                                                     |

### 2. [ GET ] Testset Status

테스트 세트의 진행상황 확인

[ Request ]

```
GET /openapi/v2/testset/{testset_id}

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request URL Parameter

| Key        | Type            | Description             | Required |
| :--------- | :-------------- | :---------------------- | :------- |
| testset_id | Positive number | 테스트 세트의 고유 번호 | Required |

[ Request Example ]

```
curl --request GET \
     --user {user_id}:{access_key} \
     https://api.apptest.ai/openapi/v2/testset/55716
```

[ Response ]

Response Body Data Type : JSON

| Key                   |                  | Type            | Description                                       |
| :-------------------- | :--------------- | :-------------- | :------------------------------------------------ |
| testset_status        |                  | String          | 테스트 세트의 진행 상태 ( Complete or Running )   |
| testset_status_detail |                  | JSON            | 테스트의 상태별 갯수                              |
|                       | total_test_cnt   | Positive number | 전체 단일 테스트 갯수                             |
|                       | error_cnt        | Positive number | 에러가 발생된 단일 테스트의 갯수 (Error)          |
|                       | fail_cnt         | Positive number | 실패한 단일 테스트의 갯수 (Fail)                  |
|                       | initializing_cnt | Positive number | 준비단계의 단일 테스트 갯수 (Initializing)        |
|                       | pass_cnt         | Positive number | 정상종료된 단일 테스트의 갯수 (Pass)              |
|                       | running_cnt      | Positive number | 테스트가 진행중인 단일테스트의 갯수 (Running)     |
|                       | stop_cnt         | Positive number | 사용자에 의해 중지된 단일 테스트의 갯수 (stopped) |
| response_time         |                  | Datetime        | 응답 시간 (timezome : UTC)                        |

[Response Example ]

```
{
    "data": {
        'testset_status': 'Complete',
        'testset_status_detail': {
            'total_test_cnt': 3,
            'error_cnt': 1,
            'fail_cnt': 1,
            'initializing_cnt': 0,
            'running_cnt': 1,
            'stop_cnt': 0,
            'pass_cnt': 0
        },
        'response_time': 'Mon, 25 May 2020 06:40:24 GMT'
    },
    "result_code": 0,
    "result_msg": "",
    "reason": ""
}
```

[ Error Code ]

| result_code | result_msg               | reason                                             |
| :---------- | :----------------------- | :------------------------------------------------- |
| 5002        | Get Testset Status Error | Global Exception Error – {ERROR_MSG}               |
| 8000        | System Maintenance       | The Service is currently under system maintenance. |

### 3. [ GET ] Testset Result

테스트가 완료되었을 경우에만 호출이 가능한 API 입니다. 테스트 세트의 결과 데이터를 조회합니다.

[ Request ]

```
GET /openapi/v2/testset/{testset_id}/result

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request URL Parameter

| Key        | Type            | Description            | Required |
| :--------- | :-------------- | :--------------------- | :------- |
| testset_id | Positive number | 테스트 세트의 고유번호 | Required |

[ Request Example ]

```
curl --request GET \
     --user {user_id}:{access_key} \
     https://api.apptest.ai/openapi/v2/testset/55716/result
```

[ Response ]

Response Body Data Type : JSON

| Key         | Type    | Description                                                   |
| :---------- | :------ | :------------------------------------------------------------ |
| complete    | Boolean | 테스트가 진행중인지 종료되었는지에 대한 여부 ( true or false) |
| result_xml  | Boolean | JUnit형식의 XML 결과 데이터                                   |
| result_html | Boolean | HTML형식의 결과 데이터                                        |
| result_json | Boolean | JUnit 형식의 Json 결과 데이터                                 |

[ Response Example ]

```
{
    "data": {
        "complete": true,
        "result_html": "",
        "result_xml": "",
        "result_json": "",
    },
    "result_code": 0,
    "result_msg": 0,
    "reason": ""
}
```

[ JUnit XML Format ]

| Key         | Type    | Description                                    |
| :---------- | :------ | :--------------------------------------------- |
| complete    | Boolean | Whether the test is running or completed (true | false) |
| result_xml  | String  | Result data in XML format in JUnit format      |
| result_html | String  | Result data in HTML format                     |
| result_json | String  | Result data in JSON format in JUnit format     |

[ JUnit XML Format Example ]

```
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="TestBot Test">
    <testsuite name="{PROJECT NAME}.TestBot">
        <testcase name="{DEVICE NAME}" time="{TEST TIME SECONDS}">
            <system-out>{RESULT PAGE LINK URL}</system-out>
        </testcase>
        <testcase name="{DEVICE NAME}" time="{TEST TIME SECONDS}">
            <error message="{RESULT PAGE LINK URL}" />
        </testcase>
    </testsuite>
</testsuites>
```

[ Error Code ]

| result_code | result_msg                     | reason                                                         |
| :---------- | :----------------------------- | :------------------------------------------------------------- |
| 5003        | Get Testset Result Data Error  | Global Exception Error – {ERROR_MSG}                           |
| 6002        | Get Testset Result Data Failed | Test is running yet. Please request when the test is complete. |
| 6003        | Data Does Not Exist            | Test data does not exist.                                      |
| 8000        | System Maintenance             | The Service is currently under system maintenance.             |

## Appendix. API Result Codes

| result_code | result_msg                     | reason                                                                                                                                                                 |
| :---------- | :----------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 4000        | Missing Request Parameter      | The required parameter was not found in the request parameters : ({PARAMETER KEY})                                                                                     |
| 4030        | App File Analysis Failed       | iOS plistlib Parsing Error : {PARSING ERROR MSG}                                                                                                                       |
| 4031        | App File Analysis Failed       | Test creation failed : There was a problem pre-processing your app file. \n the IPA file must be a development version that is signed by your development certificate. |
| 4032        | App File Analysis Failed       | Test creation failed : Invalid app file. \n There was a problem pre-processing your app file.                                                                          |
| 4040        | App File Analysis Failed       | Android Manifest Parsing Error : {PARSING ERROR MSG}                                                                                                                   |
| 4041        | App File Analysis Failed       | No launchable activity found. (android.intent.category.LAUNCHER)                                                                                                       |
| 4050        | Invalid Request Parameter      | File extension not supported. (support : ipa, zip, app, apk, xapk, apks)                                                                                               |
| 4051        | Invalid Request Parameter      | The required parameter was not found in the request parameters: (app_file)                                                                                             |
| 4052        | Invalid Request Parameter      | Invalid request parameter : (project_id : {PROJECT_ID})                                                                                                                |
| 4053        | Invalid Request Parameter      | Invalid request parameter : (testset_id : {TESTSET_ID})                                                                                                                |
| 4055        | Invalid Request Parameter      | Device is not compatible with this app : (App os type : {APP_OS_TYPE})                                                                                                 |
| 4080        | Invalid Request Parameter      | Device info does not exist in preset project data.                                                                                                                     |
| 4081        | Invalid Request Parameter      | Available devices does not exist in preset project data.                                                                                                               |
| 5001        | Run New Testset Error          | Global Exception Error – {ERROR_MSG}                                                                                                                                   |
| 5002        | Get Testset Status Error       | Global Exception Error – {ERROR_MSG}                                                                                                                                   |
| 5003        | Get Testset Result Data Error  | Global Exception Error – {ERROR_MSG}                                                                                                                                   |
| 6001        | App File Upload Error          | App file upload fail.                                                                                                                                                  |
| 6002        | Get Testset Result Data Failed | Test is running yet. Please request when the test is complete.                                                                                                         |
| 6003        | Data Does Not Exist            | Test data does not exist.                                                                                                                                              |
| 8000        | System Maintenance             | The Service is currently under system maintenance.                                                                                                                     |
