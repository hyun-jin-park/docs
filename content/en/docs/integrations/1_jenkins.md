---
title: "1. Jenkins"
date: 2020-05-15
weight: 4
description: >
#   Automating Android and iOS app testing with a jenkins pipeline
---

## apptest.ai Integration for Jenkins

##### Automating Android and iOS app testing with a jenkins pipeline

This  document explains how to configure Jenkins and use our APIs to automatically run apptest.ai tests from the build phase.

Please refer to the link [Jenkins Setup Guide](https://jenkins.io/doc/pipeline/tour/getting-started/) for Jenkins installations

### 1. Apptest.ai – Integration API

<span style="background-color: #5cb85c;padding: 5px 10px;border-radius: 5px;color: #fff;margin-right: 10px;"> POST </span> 
<span style="background-color: #f9f2f4;color: #c7254e;padding: 5px 10px;border-radius: 5px;">https://api.apptest.ai/tests_set/queuing?access_key={ACCESS_KEY}</span>

JSON  Data parameters

| Name	| Type	| Required / Optional	| Description |
|--  |--    |--   |--   |
| callback	| String	| Optional	| A callback URL, triggered when testing is complete |
| pid	| Integer	| Required	| The existing project ID in apptest.ai |
| test_set_name	| String	| Required	| A test name to be created in the apptest.ai project |
| login_id	| String	| Optional	| The ID of the account to enter When Testbot encounter Sign in screen. |
| login_pw	| String	| Optional	| The password of the account to enter when Testbot encounter Sign in screen, |
| use_vo	| Integer (0 or 1)	| Optional	| An option to enable / disable the AT&T Video Optimizer (ARO) for testing 

File Parameters

| Name	| Type	| Required / Optional	| Description |
|-- |-- |-- |--|
| apk_file	| File	| Required	| App binary file for the app to the tested 

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

The testing result in the JUnit XML Format is returned using the callback URL

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

To integrate apptest.ai into a Jenkins pipline, an access key and a project ID are required.

- How to find the access key: An access key is automatically issued when you sign up with apptest.ai. You can locate it in the apptest.ai profile page
{{< figure src="/images/1_jenkins_1.png" >}}
{{< figure src="/images/1_jenkins_2.png" >}}

- How to find the project ID: A project ID is assigned when you create a testing project
{{< figure src="/images/1_jenkins_3.png" >}}
{{< figure src="/images/1_jenkins_4.png" >}}

By default, a Sample Test Project Page is created automatically once you sign in.

Changing the settings in the Sample Test Project is not supported. However, you can change the settings for your new projects.



### 3. Jenkins – Webhook Step Plugin Installation

Search and install the “Webhook step” Plugin in the Jenkins dashboard: [Manage Jenkins] -> [Manage Plugins] -> [Available]

Skip this step if you are using the apptest.ai Test Stage Code2 source code that uses polling instead of webhook in the next stage.



### 4. Jenkins – Pipeline configuration

This section demonstraters how to connect an apptest.ai Test stage to a Jenkins pipeline item. A Jenkins pipeline item is must be already created.

Please refer to the Example Link for more detail.

- Go to the setup page and click on the Configure page.
{{< figure src="/images/1_jenkins_5.png" >}}
{{< figure src="/images/1_jenkins_6.png" >}}

- On the pipeline definition page, add the apptest.ai Test Stage Code to the Script box
{{< figure src="/images/1_jenkins_7.png" >}}

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
You can change the following items in the above script.

- accessKey: The access key from apptest.ai
- serviceProjectId: The project ID created in apptest.ai
    - Testing is performed with preset devices within a time limit defined in the apptest.ai project configuration.
    - Changing the settings for the Sample Test Project is not allowed, but you can change the settings in a new project.
- apkFile: The app path (App Binary File) to be tested

Click “Build Now” in Jenkins to start the pipeline.
{{< figure src="/images/1_jenkins_8.png" >}}

### 5. Test Results
Once the testing is complete, the testing results in the JUnit XML result format are automatically passed onto Jenkins with a callback URL. Jenkins reflects the returned testing rsults.

For more detailed analysis, please visit [apptest.ai](https://apptest.ai)

- View Testing Results in Jenkins
{{< figure src="/images/1_jenkins_9.png" >}}
{{< figure src="/images/1_jenkins_10.png" >}}
{{< figure src="/images/1_jenkins_11.png" >}}
{{< figure src="/images/1_jenkins_12.png" >}}
{{< figure src="/images/1_jenkins_13.png" >}}
{{< figure src="/images/1_jenkins_14.png" >}}

- View Testing Results in apptest.ai
{{< figure src="/images/1_jenkins_15.png" >}}
{{< figure src="/images/1_jenkins_16.png" >}}
{{< figure src="/images/1_jenkins_17.png" >}}
{{< figure src="/images/1_jenkins_18.png" >}}






