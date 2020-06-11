---
title: "3. Circle CI"
date: 2020-05-15
type: docs
weight: 4
description: >
#   A short lead descripton about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
---

## apptest.ai Integration for Circle CI

Starting with CircleCI version 2.1, the build script sharing method called orb allows you to have Testbot test your app and report the results in every build with just a few lines of scripts.

To enable the apptest.ai service during a build in CircleCI: 1. Register the apptest.ai access key in the build environment variable.

### Adding apptest.ai access key onto the build environment
1. Copy the access key string from the apptest.ai service profile page
   {{< figure src="../../images/3_circleci_1.gif" >}}


2. Connect to CircleCI and click the Setting icon to the right of the build project name to go to the build configuration page.
   {{< figure src="../../images/3_circleci_2.png" >}}


3. From the Setup page, select Environment variables menu and click Add Variables button to create the variable APPTEST_AI_ACCESS_KEY.
   {{< figure src="../../images/3_circleci_3.png" >}}


   Enter the value of the variable as “apptest.ai ID: apptest.ai Access Key”. For example, if the ID is jean@apptest.ai and the access key is “ab8f3e321d631c84c9b1113”. Register one as follows.

   ```
   jean@apptest.ai:ab8f3e321d631c84c9b1113
   ```

   The following is the normal registration and access_key is masked and printed.

### Adding apptest.ai test step to the build
1. Include apptest.ai Orb

   If the app you want to test is in Android, include android-app-test orb as shown below.

   ```
   orbs:
      apptest-ai: apptestai/android-app-test@volatile
   ```

   To test your iOS App, include ios-app-test orb as shown below.

   ```
   orbs:
      apptest-ai: apptestai/ios-app-test@volatile
   ```

2. Adding apptest.ai test build step

   You can add apptest-ai test steps anywhere in your build workflow.

   ```
   - apptest-ai/run-test:
      binary_path: app/build/outputs/apk/debug/app-debug.apk
      project_id: '818'
   ```

   > At least two arguments, binary_path and project_id, are required to invoke the service. binary_path is the path to the binary you want to test: * .apk files for android and * .ipa files for iOS. project_id is the ID for apptest.ai that contains the settings such as test time and device list for testing. The Project ID of apptest.ai can be found on the Project Information page as shown below.
   {{< figure src="../../images/3_circleci_4.png" >}}


3. Execute the build

   The apptest-ai/run-test step sends a request to test with apptest.ai and after the test is complete, prints the test result to the console as shown below.
   {{< figure src="../../images/3_circleci_5.png" >}}


   > If you want to move on to the next build step without waiting for the apptest.ai test result, you can set FALSE to the waiting_for_test_results argument as shown below.

   ```
   - apptest-ai/run-test:
      binary_path: app/build/outputs/apk/debug/app-debug.apk
      project_id: '818'
      waiting_for_test_results: 'false'
   ```

4. (Optional) Storing test result

   apptest.ai test result printed in the console is saved under “test-results” directory by default. To include the test results in the Test Summary provided by CircleCI, add the store-test-results step provided by CircleCI and specify the test result path of apptest.ai as shown below.

   ```
   - store_test_results:
      path: test-results
   ```

   If you add the store-test result step, CircleCI analyzes the test result of apptest.ai along with other test results in the Test Summary tab as below.
   {{< figure src="../../images/3_circleci_6.png" >}}


5. (Optional) Storing test result

   Because Test Summary only provides very limited pass/fail information, apptest.ai provides a more detailed test result report in a separate HTML format. To save this report in the Artifact section of CircleCI, add the store_artifacts step provided by CircleCI as below and specify the path where test results of apptest.ai are saved.

   ```
   - store_artifacts:
      path: test-results
   ```

   Saved test report is inquired as below in Artifacts tab in CircleCI.
   {{< figure src="../../images/3_circleci_7.png" >}}


   If you click this html file, you can see the detailed report as below.
   {{< figure src="../../images/3_circleci_8.png" >}}



   > If you want to change the directory that stores the test results, you can pass the path to the test_result_path argument when you run the apptest.ai test step as below.

   ```
   - apptest-ai/run-test:
      binary_path: app/build/outputs/apk/debug/app-debug.apk
      project_id: '818'
      test_result_path: app/build/outputs/apk/debug
   ```

   Finally, here’s the sample the final .circleci / config.yml file. Below is for Android.

   {{< gist genie-park cc0804ab72097afffc718defe69b83e9 >}}


   And iOS:

   {{< gist genie-park 078f9d25e81f75a2d61435c3684367f9 >}}
