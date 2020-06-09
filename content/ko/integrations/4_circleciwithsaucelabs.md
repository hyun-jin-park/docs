---
title: "4. Circle CI with Saucelabs"
date: 2020-05-15
weight: 4
description: >
#   A short lead descripton about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
---

## SauceLabs에서 Circle CI와 apptest.ai 모두 사용하기

CircleCI에서 apptest.ai 서비스를 사용하여 스크립트를 작성없이 스모크 테스트(Smoke Test)를 Daily 빌드마다 수행하는 방법에 대한 튜토리얼이 있습니다. 이번에는 apptest.ai에서 제공하는 장치가 아니라 Sauce Labs에서 제공하는 장치를 사용하여 동일하게 테스트하는 방법을 소개합니다.

CircleCI가 appTest.ai 서비스를 사용하는 방식과 다른 점은 apptest.ai의 액세스 키 대신 Sauce Labs API 키를 사용하고 apptest.ai 및 Sauce Labs 계정을 연결하는 것입니다. 현재 베타서비스로 운영중이며, apptest.ai 및 Sauce Labs 계정을 연결하려면 [contact@apptest.ai] (mailto : contact@apptest.ai)로 연락주시기 바랍니다.


1. 아래와 같이 Sauce Labs 서비스에 액세스하여 API 키를 얻습니다.
  {{< figure src="/images/4_saucelabs_1.png" >}}

2. 이전에 CircleCI에 apptest.ai 액세스 키를 등록한 것과 같은 방법으로 Sauce Labs API를 사용하여 CircleCI에서 APPTEST_AI_ACCESS_KEY를 등록하십시오
  {{< figure src="/images/4_saucelabs_2.png" >}}


3. Sauce Labs 기반 apptest.ai 테스트 단계 orb를 CircleCI의 빌드 구성 파일 (circleci / config, yml)에 추가합니다. orb의 이름이 apptestai/android-app-test에서 apptestai/android-app-test-saucelabs로 바뀌 었다는 점을 제외하면 마찬가지입니다. 즉, 아래와 같이 orb를 입력해 줍니다.
  ```
  orbs:
    apptest-ai: apptestai/android-app-test-saucelabs@dev:0.1.9
  ```

  apptest.ai 테스트 단계를 추가하여 다음과 같이 테스트를 시작하십시오.
  ```
  - apptest-ai/run-test:
      binary_path: app/build/outputs/apk/debug/app-debug.apk
  ```

4. 빌드시 아래와 같이 Sauce Labs 계정에 등록 된 장치로 테스트 할 수 있으며, 결과는 CircleCI의 아티팩트 탭에 저장됩니다. Sauce Labs에 액세스하면 장치 로그를 포함한 자세한 테스트 데이터에 액세스 할 수 있습니다.
  {{< figure src="/images/4_saucelabs_3.png" >}}


    Sauce Labs에서 제공 한 LG G5 장치에서의 테스트가 성공적으로 완료되었습니다
    {{< figure src="/images/4_saucelabs_4.png" >}}


    CircleCI의 테스트 Summary는 테스트 봇의 결과도 보여줍니다.
    {{< figure src="/images/4_saucelabs_5.png" >}}


    CircleCI 아티팩트에서 HTML로 저장된 테스트 보고서를 확인하실 수 있습니다.
    {{< figure src="/images/4_saucelabs_6.png" >}}


    이 HTML을 열어 자세한 테스트 결과를 볼 수 있습니다.
    {{< figure src="/images/4_saucelabs_7.png" >}}


    테스트에 사용 된 LG G5 보고서의 세부 사항을 보려면 Sauce Labs에 접속하시면 됩니다.
    {{< figure src="/images/4_saucelabs_8.png" >}}


    SauceLabs에서는 CircleCI가 요청한 테스트 중에 수집 된 스크린 샷을 볼 수 있습니다.
    {{< figure src="/images/4_saucelabs_9.png" >}}


    SauceLabs에서는 CircleCI에서 테스트 중에 수집 된 자세한 장치 로그를 검색 할 수도 있습니다.


    #### 마지막으로, 이 데모에 표시된 CircleCI에서 사용하는 전체 빌드 스크립트가 아래에 나와 있습니다.

    {{< gist genie-park 21d914c38dcc6f7f81cfe95b89a1969c >}}

