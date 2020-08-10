---
title: "1. Jenkins"
date: 2020-05-15
weight: 4
description: >

#   A short lead descripton about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
---

## Jenkins 사용자를 위한 apptest.ai 통합 가이드

##### Jenkins 파이프라인으로 Android 및 iOS 앱 테스트 자동화하기

이 문서는 Jenkins를 구성하고 API를 사용하여 빌드 단계에서 apptest.ai 테스트를 자동으로 실행할 수 있도록 통합시키는 방법을 설명합니다.

Jenkins 설치에 대해서는 [Jenkins Setup Guide](https://jenkins.io/doc/pipeline/tour/getting-started/) 링크를 참조하시기 바랍니다.

### 1. Apptest.ai – Integration API

<span style="background-color: #5cb85c;padding: 5px 10px;border-radius: 5px;color: #fff;margin-right: 10px;"> POST </span>
<span style="background-color: #f9f2f4;color: #c7254e;padding: 5px 10px;border-radius: 5px;">https://api.apptest.ai/tests_set/queuing?access_key={ACCESS_KEY}</span>

JSON Data parameters

| Name          | Type             | Required / Optional | Description                                                  |
| :------------ | :--------------- | :------------------ | :----------------------------------------------------------- |
| callback      | String           | Optional            | 테스트가 완료되면 호출될 Callback URL                        |
| pid           | Integer          | Required            | apptest.ai에서 생성한 프로젝트의 고유번호                    |
| test_set_name | String           | Required            | apptest.ai의 프로젝트에 생성될 테스트 세트의 이름            |
| login_id      | String           | Optional            | Testbot이 로그인 화면을 만났을때 입력할 앱의 로그인 아이디   |
| login_pw      | String           | Optional            | Testbot이 로그인 화면을 만났을때 입력할 앱의 로그인 비밀번호 |
| use_vo        | Integer (0 or 1) | Optional            | AT&T사의 Video Optimizer (ARO)를 활성화 / 비활성화하는 옵션  |

File Parameters

| Name     | Type | Required / Optional | Description             |
| :------- | :--- | :------------------ | :---------------------- |
| apk_file | File | Required            | 테스트 대상이 될 앱파일 |

Example Response

```
{
  "data": {
    "test_count": 1,      // Test Count (Integer),
    "tsid": 192948        // Test Set ID (Integer)
  },
  "errorCode": 0,
  "reason": "",
  "result": "ok"
}
```

Callback result data format

Callback URL을 통해 리턴되는 JUnit XML 형식의 테스트 결과

```
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="TestBot Test">
    <testsuite name="TestBot Test.Apk File Name (with Version)">
      <testcase name="Device Name 1" time="Test Duration (sec)">
      </testcase>
      <testcase name="Device Name 2" time="Test Duration (sec)">
        <error message="apptest.ai Result Page Link"></error>
      </testcase>
      ...
    </testsuite>
</testsuites>
```

<span style="background-color: #5cb85c;padding: 5px 10px;border-radius: 5px;color: #fff;margin-right: 10px;"> GET </span>
<span style="background-color: #f9f2f4;color: #c7254e;padding: 5px 10px;border-radius: 5px;">https://api.apptest.ai/tests_set/{tsid}/ci_info?access_key=ACCESS_KEY</span>

```
Response
{
    "complete": true,
    "data": {
        "result_xml":  "<?xml version="1.0" encoding="UTF-8"?> <testsuites name="TestBot Test"> <testsuite name="TestBot Test.Apk File Name (with Version)"> <testcase name="Device Name 1" time="Test Duration (sec)"> </testcase> <testcase name="Device Name 2" time="Test Duration (sec)"> <error message="Apptes.ai Result Page Link"></error> </testcase> ... </testsuite> </testsuites>"
    },
    "errorCode": 0,
    "reason": "",
    "result": "ok"
}
```

### 2. apptest.ai – Access Key and Project ID

apptest.ai를 Jenkins 파이프 라인에 통합하려면 액세스 키와 프로젝트의 고유번호(Project ID)가 필요합니다.

- 액세스 키를 찾는 방법 : apptest.ai에 가입하면 액세스 키가 자동으로 발급됩니다. apptest.ai Profile 페이지에서 확인하실 수 있습니다.
  {{< figure src="../../../images/1_jenkins_1.png" >}}
  {{< figure src="../../../images/1_jenkins_2.png" >}}

- 프로젝트 ID를 찾는 방법 : 테스트 프로젝트를 만들 때 프로젝트 ID가 할당됩니다.
  {{< figure src="../../../images/1_jenkins_3.png" >}}
  {{< figure src="../../../images/1_jenkins_4.png" >}}

  회원가입시 기본적으로 샘플 테스트 프로젝트가 1개 생성됩니다.

  샘플 테스트 프로젝트의 설정 변경은 지원되지 않습니다. 그러나 새로 생성한 프로젝트는 설정을 변경할 수 있습니다.

### 3. Jenkins – Webhook Step Plugin Installation

Jenkins 대시 보드에서 "Webhook step" 플러그인을 검색하여 설치하십시오. [Jenkins 관리]-> [플러그인 관리]-> [사용 가능]

다음 단계에서 webhook 대신 폴링을 사용하는 apptest.ai Test Stage Code2 소스 코드를 사용하는 경우이 단계를 건너 뛰십시오.

### 4. Jenkins – Pipeline configuration

이 섹션에서는 apptest.ai Test 스테이지를 Jenkins 파이프 라인 항목에 연결하는 방법을 보여줍니다. Jenkins 파이프 라인 항목이 이미 작성되어 있어야합니다.

자세한 내용은 예제 링크를 참조하십시오.

- 메인 페이지에서 좌측메뉴의 Configure 버튼을 클릭하십시오.
  {{< figure src="../../../images/1_jenkins_5.png" >}}
  {{< figure src="../../../images/1_jenkins_6.png" >}}

- 파이프 라인 설정 페이지에서 apptest.ai Tes Stage Code를 스크립트 입력란에 추가하십시오.
  {{< figure src="../../../images/1_jenkins_7.png" >}}

  ##### [apptest.ai Test Stage Code 1] – Webhook

  ```
  import groovy.json.JsonSlurper
  node {
      def gitRepository
      def androidHome
      def apkFile
      def accessKey
      def serviceProjectId
      def asyncResult
      def serviceUrl
      def serviceHost
      def serviceCompleteCheckUrl

      // Add to your Preparation Stage
      stage('Preparation') {

          // Git Repository URL Address
          gitRepository = 'https://github.com/forcemax/apps-android-wikipedia.git'
          // Android Home Path
          androidHome = '/opt/apps/android-sdk'

          echo "Current workspace : ${workspace}"
          accessKey = 'e40c4487b3cf5e6d53ef0543d2614488'
          serviceProjectId = 376
          serviceHost = "https://api.apptest.ai"
          serviceUrl = "${serviceHost}/test_set/queuing?access_key=${accessKey}"

      }

      // Git Source Clone Stage
      stage('Clone source') {
          git branch: 'master', url: gitRepository
      }
      // Android SDK directory path Setting
      stage('SDK Configure') {
          sh "echo 'sdk.dir=${androidHome}' > local.properties"
      }
      // Gradle AssembleDebug Build Stage
      stage('Gradle Debug build') {
          sh "./gradlew :app:assembleDebug"
      }
  ```


        // apptest.ai Test Stage
        stage('Apptestai Test') {
            apkFile="path/to/file/your_app.apk"
            hook = registerWebhook()

            // Call apptest.ai's Test API.
            // Refer to 1. API Spec
            r = sh(returnStdout: true, script: "curl -X POST -F 'apk_file=@\"${apkFile}\"' -F 'data={\"pid\": ${serviceProjectId}, \"test_set_name\": \"${env.BUILD_TAG}\", \"callback\": \"${hook.getURL()}\"}' ${serviceUrl}").toString().trim()
            j = new JsonSlurper().parseText(r)
            echo "jsonParse Result: ${j}"
            if (j.errorCode != 0) {
                echo "apptest.ai Test cannot started. ${r}"
                r = null
                j = null
                error "FAIL"
            }

            tsid=j['data']['tsid']
            echo "apptest.ai TestSet id : ${tsid}"

            echo "Waiting for POST to ${hook.getURL()}"

            // Wait for Test Complete signal
            waitForWebhook hook

            // Get the Test Result Data
            // Refer to 1. API Spec
            serviceCompleteCheckUrl = "${serviceHost}/test_set/${tsid}/ci_info?access_key=${accessKey}"
            r = sh( returnStdout: true, script: "curl -X GET ${serviceCompleteCheckUrl}").toString().trim()
            j = new JsonSlurper().parseText(r)
            asyncResult = j.data.result_xml

            echo "Webhook called with data: ${asyncResult}"
        }

        stage('Write Apptestai Test Result') {
            sh "mkdir -p tmp/"
            // Write File file:"tmp/TESTS-Apptestai.xml", text: asyncResult, encoding: "UTF-8"
            sh "echo -n '${asyncResult}' > tmp/TESTS-Apptestai.xml"
        }
        stage('jUnit Test') {
            junit 'tmp/TESTS-*.xml'
        }
    }
    ```

    ##### [apptest.ai Test Stage Code 2] – Polling
    ```
    import groovy.json.JsonSlurper
    node {
        def gitRepository
        def androidHome
        def apkFile
        def accessKey
        def serviceProjectId
        def testResult
        def serviceUrl
        def serviceHost
        def serviceCompleteCheckUrl

        // Add to your Preparation Stage
        stage('Preparation') {

            // Git Repository URL Address
            gitRepository = 'https://github.com/forcemax/apps-android-wikipedia.git'
            // Android Home Path
            androidHome = '/opt/apps/android-sdk'

            echo "Current workspace : ${workspace}"
            accessKey = 'e40c4487b3cf5e6d53ef0543d2614488'
            serviceProjectId = 376
            serviceHost = "https://api.apptest.ai"
            serviceUrl = "${serviceHost}/test_set/queuing?access_key=${accessKey}"

        }

        // Git Source Clone Stage
        stage('Clone source') {
            git branch: 'master', url: gitRepository
        }
        // Android SDK directory path Setting
        stage('SDK Configure') {
            sh "echo 'sdk.dir=${androidHome}' > local.properties"
        }
        // Gradle AssembleDebug Build Stage
        stage('Gradle Debug build') {
            sh "./gradlew :app:assembleDebug"
        }


        // Apptest.ai Test Stage
        stage('apptestai Test') {
            apkFile="path/to/file/your_app.apk"
            // Call apptest.ai\'s Test API.
            r = sh(returnStdout: true, script: "curl -X POST -F 'apk_file=@\"${apkFile}\"' -F 'data={\"pid\": ${serviceProjectId}, \"test_set_name\": \"${env.BUILD_TAG}\"}' ${serviceUrl}").toString().trim()
            j = new JsonSlurper().parseText(r)
            echo "jsonParse Result: ${j}"
            if (j.errorCode != 0) {
                echo "apptest.ai Test cannot started. ${r}"
                r = null
                j = null
                error "FAIL"
            }

            tsid=j['data']['tsid']
            echo "Apptest.ai TestSet id : ${tsid}"

            // Get the Test Result Data
            // Refer to 1. API Spec
            serviceCompleteCheckUrl="${serviceHost}/test_set/${tsid}/ci_info?access_key=${accessKey}"

            r = null
            j = null

            waitUntil {
                sleep (time: 10, unit: "SECONDS")
                r = sh( returnStdout: true, script: "curl -X GET ${serviceCompleteCheckUrl}").toString().trim()
                j = new JsonSlurper().parseText(r)
                complete=j.complete
                echo "ci_info result : ${complete}"
                if (complete == true) {
                    testResult=j.data.result_xml
                    r = null
                    j = null
                    return true
                }

                r = null
                j = null
                return false
            }

            echo "test complete with data: ${testResult}"
        }

        stage('Write Apptestai Test Result') {
            sh "mkdir -p tmp/"
            // Write File file:"tmp/TESTS-Apptestai.xml", text: testResult, encoding: "UTF-8"
            sh "echo -n '${testResult}' > tmp/TESTS-Apptestai.xml"
        }
        stage('jUnit Test') {
            junit 'tmp/TESTS-*.xml'
        }
    }
    ```

    위 스크립트에서 다음 항목들을 수정하십시오.

    - accessKey: apptest.ai의 액세스 키
    - serviceProjectId: apptest.ai에 생성된 프로젝트의 고유번호 (Project ID)
        - apptest.ai 프로젝트 설정에 저장된 Time Limit과 디바이스 정보를 적용하여 테스트를 수행합니다.
        - 샘플 테스트 프로젝트의 설정 변경은 허용되지 않지만 새 프로젝트의 설정은 변경할 수 있습니다.
    - apkFile: 테스트 할 대상 앱파일(App Binary File)

    파이프 라인을 시작하려면 Jenkins에서 "Build Now"를 클릭하십시오.
    {{< figure src="../../../images/1_jenkins_8.png" >}}

### 5. Test Results

테스트가 완료되면 JUnit XML 결과 형식의 테스트 결과가 Callback URL을 통해 Jenkins에 자동으로 전달됩니다. Jenkins는 반환 된 테스트 결과를 반영합니다.

자세한 테스트결과 분석 정보는 [apptest.ai](https://apptest.ai)를 방문해 확인하실 수 있습니다.

- Jenkins에서 테스트 결과보기
  {{< figure src="../../../images/1_jenkins_9.png" >}}
  {{< figure src="../../../images/1_jenkins_10.png" >}}
  {{< figure src="../../../images/1_jenkins_11.png" >}}
  {{< figure src="../../../images/1_jenkins_12.png" >}}
  {{< figure src="../../../images/1_jenkins_13.png" >}}
  {{< figure src="../../../images/1_jenkins_14.png" >}}

- apptest.ai 에서 테스트 결과보기
  {{< figure src="../../../images/1_jenkins_15.png" >}}
  {{< figure src="../../../images/1_jenkins_16.png" >}}
  {{< figure src="../../../images/1_jenkins_17.png" >}}
  {{< figure src="../../../images/1_jenkins_18.png" >}}
