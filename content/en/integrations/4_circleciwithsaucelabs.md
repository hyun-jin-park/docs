---
title: "4. Circle CI with Saucelabs"
date: 2020-05-15
type: docs
weight: 4
description: >
#   A short lead descripton about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
---

## Using apptest.ai with Circle CI on SauceLabs

We have a tutorial on how to use the apptest.ai service on CircleCI to perform a daily build which is running a smoke test without writing a single line of code. This time, I will introduce how to test the same using a device provided by Sauce Labs, not a device provided by apptest.ai.

The only change from the way the CircleCI used the appTest.ai service is to use Sauce Labs API Keys instead of apptest.ai’s access keys, and to associate the apptest.ai and Sauce Labs accounts. If you are interested in linking your apptest.ai and Sauce Labs accounts during the current Beta period, please send an email to [contact@apptest.ai](mailto:contact@apptest.ai)

1. Access to Sauce Labs service as below and obtain your API key
{{< figure src="/images/4_saucelabs_1.png" >}}

2. Register the APPTEST_AI_ACCESS_KEY in CircleCI using Sauce Labs API the same way that your previously registered the apptest.ai access key in CircleCI
{{< figure src="/images/4_saucelabs_2.png" >}}


3. Add Sauce Labs-based apptest.ai Test Step orb to CircleCI’s build configuration file (circleci/config,yml). The same is true except that orb has been renamed from apptestai/android-app-test to apptestai/android-app-test-saucelabs. In other words, include orb as below:
```
orbs:
  apptest-ai: apptestai/android-app-test-saucelabs@dev:0.1.9
```

Add apptest.ai test step to start testing as below
```
 - apptest-ai/run-test:
    binary_path: app/build/outputs/apk/debug/app-debug.apk
```

4. When you build, you can test with the device registered in your Sauce Labs account as below, and the result is saved in the Artifacts tab in CircleCI. More detailed test data including device log can be accessed by accessing Sauce Labs.
{{< figure src="/images/4_saucelabs_3.png" >}}


Test on LG G5 provided by Sauce Labs has been successfully completed
{{< figure src="/images/4_saucelabs_4.png" >}}


The test summary of CircleCI also demonstrates the result the testbot
{{< figure src="/images/4_saucelabs_5.png" >}}


You can find the test report saved in HTML from in CircleCI Artifacts.
{{< figure src="/images/4_saucelabs_6.png" >}}


You can open this HTML to see the detailed test results
{{< figure src="/images/4_saucelabs_7.png" >}}


Connect to Sauce Labs to see the details of the LG G5 report used in the test
{{< figure src="/images/4_saucelabs_8.png" >}}


Sauce Labs can view the screen shots collected during the tests requested by CircleCI
{{< figure src="/images/4_saucelabs_9.png" >}}


In Sauce Labs, you can search the detailed device log collected during the test on the CircleCI


#### Finally, the full build script used by CircleCI shown in this demo is shown below.

{{< gist genie-park 21d914c38dcc6f7f81cfe95b89a1969c >}}

