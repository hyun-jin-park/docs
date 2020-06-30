---
title: "Rest APIs for CI/CD"
linkTitle: "Rest APIs"
type: docs
weight: 4
description: >
#   What does your user need to understand about your project in order to use it - or potentially contribute to it? 
---

## Integrating REST APIs into CI / CD pipelines

This is API information provided to use apptest.ai in connection with CI(Continuous Integration) tools. You can use apptest.ai through test set creation, test status view, and test result view API.

### 1. [POST] Run New Testset

Run a New Testset. (Testset is group of unit Tests)<br/>
Some Options of the new test run follow the configuration information stored in the project.

[Request]
```
POST /openapi/v2/testset

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request Body Miltipary Form Data #1

| key       | type  | Description               | Required  |
|:--        |:--    |:--                        |:--        |
| app_file  | File  | Target APP File to Test   |  Required  

Request Body Miltipary Form Data #2

| key           |           | type              |                       | Description    | Required |
|:--            |:--        |:--                |:--                    |:--             |:--|
| pid           |           | Positive Number   | -                     | Unique number of Project <br/> ex) 509   | Required |
| testset_name  |           | String            | Max 100 Characters    | Name of testset <br/> ex) "Testset name Example#1" | Required |
| time_limit   |           | Positive Number   | Min : 5 <br/>Max : 30 | Test time limit (Minutes) <br/>ex) 5 <br/>If the value is empty, it follows the time-limit saved in the project settings.     | Required |
| use_vo        |           | Boolean           | Default: false        | Whether AT&T Video Optimizer(ARO) is used (`true` or `false` ) <br/>ex) true  | Required |
| callback      |           | String            | Max 250 Characters    | Callback URL to be called after test completion. <br/>ex) 'https://127.0.0.1/callback/url/example'  | Required |
| credentials   | login_id  | String            | Max 150 Characters    | Account information(ID) of the test target app to be used to test the app <br/>(Test credentials info - Login ID) <br/>ex) 'test@gmail.com' or '01012345678' or 'testaccount'            | Required |
|               | login_pw  | String            | Max 150 Characters    | Account information(Password) of the test target app to be used to test the app <br/>(Test credentials info - Login PW) <br/>ex) 'password1234!'      | Required 

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
                "login_id": "test@gmail.com", \
                "login_pw": "password1234!" \
            } \
        }' https://api.apptest.ai/openapi/v2/testset
```

[ Response ]


| key           |           | type              | Description                                               |
|:--            |:--        |:--                |:--                                                        |
| test_count    |           | Positive Number   | Number of Unit tests run on the Testset  <br/> ex) 3      |
| testset_id    |           | Positive Number   | Unique number of the Testset <br/> ex) 251929             


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

| result_code   | result_msg                | reason    |
|:--            |:--                        |:--        |
| 4000          | Missing Request Parameter | The required parameter was not found in the request parameters : {{ PARAMETER KEY }} |
| 4030          | App File Analysis Failed  | iOS plistlib Parsing Error : {{ PARSING ERROR MSG }} |
| 4031          |                           | Test creation failed : There was a problem pre-processing your app file. <br /> The IPA file must be a development version that is signed by your development certificate. |
| 4032          |                           | Test creation failed. Invalid app file. <br /> There was a problem pre-processing your app file. |
| 4040          |                           | Android Manifest Parsing Error : {{ PARSING ERROR MSG }} |
| 4041          |                           | No launchable activity found. (android.intent.category.LAUNCHER) |
| 4050          | Invalid Request Parameter | File extension not supported. ( Support : ipa, zip, app, apk, xapk, apks ) |
| 4051          |                           | The required parameter was not found in the request parameters : (app_file) |
| 4052          |                           | Invalid request parameter : (project_id: {{ PROJECT_ID }}) |
| 4053          |                           | Invalid request parameter : (testset_id: {{ TESTSET_ID }}) |
| 4055          |                           | Device is not compatible with this app : ( APP os type : {{ APP_OS_TYPE }} ) |
| 4080          | Invalid Request Parameter | Device info does not exist in prese project data. |
| 4081          |                           | Available devices does not exist in preset project data. |
| 5001          | Run New Testset Error     | Global Exception Error - {{ ERROR_MSG }} |
| 6001          | App File Upload Error     | App file upload fail. |
| 8000          | System Maintenance        | The service is currently under system maintenance. 


### 2. [ GET ] Testset Status

Check the progress of the testset

[ Request ]

```
GET /openapi/v2/testset/{testset_id}

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request URL Parameter

| Key           | Type              | Description               | Required  |
|:--            |:--                |:--                        |:--        |
| testset_id    | Positive number   | Unique number of Testset  | Required  

[ Request Example ]

```
curl --request GET \
     --user {user_id}:{access_key} \
     https://api.apptest.ai/openapi/v2/testset/55716
```

[ Response ]

Response Body Data Type : JSON

| Key                   |                   | Type              | Description                               |
|:--                    |:--                |:--                |:--                                        |
| testset_status        |                   | String            | Testset Status ( Complete or Running )    |
| testset_status_detail |                   | JSON              | Count of the result for each tests.       |
|                       | total_test_cnt    | Positive number	| Total test counts                         |
|                       | error_cnt         | Positive number	| Error detected test counts                |
|                       | fail_cnt          | Positive number	| Failed test counts                        |
|                       | initializing_cnt  | Positive number	| Initializing test counts                  |
|                       | pass_cnt          | Positive number	| passed test counts                        |
|                       | running_cnt	    | Positive number	| Running test counts                       |
|                       | stop_cnt	        | Positive number	| stopped test counts                       |
| response_time	        |                   | Datetime	        | Response time (timezome : UTC)


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

| result_code   | result_msg                | reason    |
|:--            |:--                        |:--        |
| 5002          | Get Testset Status Error  | Global Exception Error – {ERROR_MSG} |
| 8000          | System Maintenance        | The Service is currently under system maintenance. 



### 3. [ GET ] Testset Result

This API that can be called only when the test is completed. Returns the result data of the testset.

[ Request ]

```
GET /openapi/v2/testset/{testset_id}/result

Host : https://api.apptest.ai

Authorization : Basic {user_id}:{access_key}
```

Request URL Parameter

| Key           | Type              | Description               | Required  | 
|:--            |:--                |:--                        |:--        |
| testset_id    | Positive number   | Unique number of Testset  | Required


[ Request Example ]

```
curl --request GET \
     --user {user_id}:{access_key} \
     https://api.apptest.ai/openapi/v2/testset/55716/result
```

[ Response ]

Response Body Data Type : JSON

| Key           | Type      | Description                                               | 
|:--            |:--        |:--                                                        |
| complete      | Boolean   | Whether the test is running or completed ( true or false) |
| result_xml    | Boolean   | Result data in XML format in JUnit format                 |
| result_html   | Boolean   | Result data in HTML format                                |
| result_json   | Boolean   | Result data in JSON format in JUnit format          


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

| Key	        | Attributes    | Description                                               |
|:--            |:--            |:--                                                        |
| testsuite     | name          | apptest.ai's project name |
| testcase      | name          | Name of the device under test (Unit Test) |
|               | time          | Time the test was performed |
| system-out    | -             | Test result link of apptest.ai |
| error         | message       | Test results link where errors were found |


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

| result_code   |	result_msg                  | reason                                                        |
|:--            |:--                            |:--                                                            |
| 5003	        | Get Testset Result Data Error	| Global Exception Error – {ERROR_MSG}                          |
| 6002	        | Get Testset Result Data Failed| Test is running yet. Please request when the test is complete.|
| 6003	        | Data Does Not Exist	        | Test data does not exist.                                     |
| 8000	        | System Maintenance	        | The Service is currently under system maintenance.



### Appendix. API Result Codes

| result_code |	result_msg | reason |
|:--    |:--                            |:--                                                        |
| 4000	| Missing Request Parameter	    | The required parameter was not found in the request parameters : ({PARAMETER KEY}) |
| 4030	| App File Analysis Failed	    | iOS plistlib Parsing Error : {PARSING ERROR MSG} |
| 4031	| App File Analysis Failed	    | Test creation failed : There was a problem pre-processing your app file. <br /> the IPA file must be a development version that is signed by your development certificate. |
| 4032	| App File Analysis Failed	    | Test creation failed : Invalid app file. <br /> There was a problem pre-processing your app file. |
| 4040	| App File Analysis Failed	    | Android Manifest Parsing Error : {PARSING ERROR MSG} |
| 4041	| App File Analysis Failed	    | No launchable activity found. (android.intent.category.LAUNCHER) |
| 4050	| Invalid Request Parameter	    | File extension not supported. (support : ipa, zip, app, apk, xapk, apks) |
| 4051	| Invalid Request Parameter	    | The required parameter was not found in the request parameters: (app_file) |
| 4052	| Invalid Request Parameter	    | Invalid request parameter : (project_id : {PROJECT_ID}) |
| 4053	| Invalid Request Parameter	    | Invalid request parameter : (testset_id : {TESTSET_ID}) |
| 4055	| Invalid Request Parameter 	| Device is not compatible with this app : (App os type : {APP_OS_TYPE}) |
| 4080	| Invalid Request Parameter	    | Device info does not exist in preset project data. |
| 4081	| Invalid Request Parameter	    | Available devices does not exist in preset project data. |
| 5001	| Run New Testset Error	        | Global Exception Error – {ERROR_MSG} |
| 5002	| Get Testset Status Error	    | Global Exception Error – {ERROR_MSG} |
| 5003	| Get Testset Result Data Error	| Global Exception Error – {ERROR_MSG} |
| 6001	| App File Upload Error	        | App file upload fail. |
| 6002	| Get Testset Result Data Failed| Test is running yet. Please request when the test is complete. |
| 6003	| Data Does Not Exist	        | Test data does not exist. |
| 8000	| System Maintenance	        | The Service is currently under system maintenance.
