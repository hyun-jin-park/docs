---
title: "User Guide"
linkTitle: "User Guide"
type: docs
weight: 4
description: >
---
# apptest.ai Website User Guide
![Logo](https://apptest.ai/wp-content/uploads/2019/10/logo_black.png)
September 2020

## Contents
* Introduction
* Definitions
* About apptest.ai
* Account Creation & Login
* Project Homepage
* Teams
* Projects
* Tests
  * Creating an App Test
  * Creating a Website Test
  * Running a Test
  * Test Results
  * Deleting a Test
* Profile Settings 
* Further Help

## Introduction
Welcome to the apptest.ai User Guide. 
This user’s guide includes the information you need to work with apptest.ai effectively. 
It contains detailed information about the following:
* Overview of apptest.ai and products offered
* How to navigate through the apptest.ai site 
* How to create and run tests
* How to understand testbot results

### Definitions
A **project** works like a portfolio that contains all your tests and test sets. Each project is assigned to a team. 
Clicking on a project will show you all the test sets contained within that project. 
Deleting a project will delete all the tests and test sets within that project.
![project](images/5-userguide-1.png)
A **test set** refers to the collection of tests executed by our AI testbot on an single app or website across multiple devices. Each app/website tested will be recorded in a unique test set. Clicking on a test set will show you all the individual tests within that test set. Deleting a test set will delete all the tests within that test set.
![test set](/images/5-userguide-2.png)
A **test** refers to a test executed by our AI testbot on an app or website for a single device. When multiple devices are chosen to test an app, test result data will be produced separately for each device. 
![test](/images/5-userguide-3.png)

### About apptest.ai
apptest.ai uses a trained AI testbot on real iOS/Android devices to explore your app and locate critical errors. We provide a detailed test report with precise navigation paths, screenshots, and test logs. This allows you to eliminate bugs before your customers use your app, leading to high customer satisfaction.   

How it works:
1. Upload your iOS/Android application
2. Select the devices to test your application
3. Allow apptest.ai testbot to explore your app and execute tests
4. Receive your detailed results report

### Account Creation & Login
To create an account, click “Try for free” or “Go to test” on the apptest.ai homepage(1). 
![account1](/images/5-userguide-4.png)
You will be directed to the login page. Select “Sign up”(2).
![account2](/images/5-userguide-5.png)
Create an account by providing your email address or signing in through Github or Google(3).
![account3](/images/5-userguide-6.png)

Please note: Email verification will be required to create your account. 

Once you have successfully created your account, you can login via the apptest.ai homepage by clicking “Try for free” or “Go to test” OR directly at app.apptest.ai.

## Projects
### Project Homepage
![Homepage](https://apptest.ai/wp-content/uploads/2020/04/6.png)
Successful login will direct you to your personal project homepage, which provides an overview of your account. The two main sections are: Dashboard and Personal Projects.

__Dashboard__
Your dashboard provides monthly account summaries for the current month and previous month. This information provides an overview of your monthly total test count and a breakdown of your monthly test results.
![dash](/images/5-userguide-7.png)

__Personal Projects__
The personal projects section highlights your 3 most recently projects at the top and also allows you to view all your projects. Projects are listed in descending order, beginning with the most recently created project. Clicking on a project will open a detailed project summary. Clicking on the 3 dots on the right side of each project allows you to edit the project settings or delete the project. 
Note: Projects will not be highlighted in the recent projects section until a test has been completed.

### Teams
Teams foster collaboration by allowing multiple apptest.ai users to work together on a Project and share Test Results.
To create a new team, click on “My Projects” in the top right of your Dashboard. From the dropdown menu, click “Create Team”. Use the pop-up window to select your team name and create your team.
![team1](/images/5-userguide-8.png)
![team2](/images/5-userguide-9.png)
To navigate between your personal homepage and different team homepages, click on “My Projects” in the top right of your Dashboard. From the dropdown menu, select on the team you would like to view.
Editing your team settings, including adding and removing members, can be done in within your profile settings.

### Create a New Project
1. Click on the plus button under “Create New Test Project”. 
![project1](/images/5_userguide_10.png)
2. Follow the prompts in the pop-up to assign the project name (e.g. Test), use the drop-down list to select which team will be in charge of the project (e.g. Personal), and then click “Create”.
![project2](/images/5_userguide_11.png)
3. Your newly created project will appear at the top of your project list on your homepage. Click on your project to open it and create new tests.
![project3](/images/5_userguide_12.png)
![project4](/images/5_userguide_13.png)

### Project Settings

Within a project page, click the settings button to update any project information including:
* Project name
* Team assignment
* Default test length (can be adjusted again individual test settings)
* Device language
* Test credentials
* Devices for testing
Additionally, you can upload a new App File to the App Repository or update you Web test settings. 
![Project Setttings 1](/images/5_userguide_14.png)
![Project Settings 2](/images/5_userguide_15.png)
![Project Settings 3](/images/5_userguide_16.png)
![Project Settings 4](/images/5_userguide_17.png)

## Tests
### Create a New Test (App)
![Create App Test](/images/5_userguide_18.png)
1. Click on the red New Test button on your project homepage or within a premade project. A pop-up will open to create your new test.
2. Upload your app. We currently offer 4 methods to upload your app:
    * Drag & Drop
    * Browse file (from your device)
    * Sample Apps
    Choose from one of our pre-uploaded sample apps to run a test.
    * App ID
    Select your App type (Android/iOS) and type out the App ID. 
3. Use the drop-down list to select which team you will have access to your test results (e.g. My Projects) and choose a name for your test (e.g. Test).
4. Use the sliding bar to choose the length of your test (minimum- 5 mins., maximum- 600 mins.) and select your device language (English/Korean).
5. Optional: If your app includes any sign in screens, provide the login ID and password in the boxes under “Test Credentials” so they can be entered during your test.
 ![Test Credentials](/images/5_userguide_19.png)
6. Click “Add more devices” on the left hand menu and select which devices you desire to run your test.
    *Note: Depending on the type of app (e.g. Android/iOS) you uploaded on the previous page, some phones may not be available for selection.
 ![Add more devices](/images/5_userguide_20.png) 
7. Click “Additional options” on the left hand menu to select whether or not you would like an additional performance report using [AT&T Video Optimizer](https://developer.att.com/video-optimizer).
 ![AT&T Video Optimizer](/images/5_userguide_21.png)
8. Click Start Test at the bottom of the pop up screen to begin your test.

### Create a New Test (Website)
 ![Create Web Test](/images/5_userguide_22.png)
1. Click on the green New Test button on the project homepage or within a premade project. A pop-up will open to create your new test.
2. Use the drop-down list to select which team you will have access to your test results (e.g. My Projects) and choose a name for your test (e.g. Test).
3. Provide your website URL. We currently offer 2 types of URL tests:
    * Wordpress URL Test
    Follow the prompts to provide your admin URL, username, password, and URL.
    * Custom URL Test
4. Use the sliding bar to choose the length of your test (minimum- 5 mins., maximum- 600 mins.).
5. Provide your browser name and use the drop-down list to select your browser resolution.
6. Optional: If your website includes any sign in screens, provide the login ID and password in the boxes under “Test Credentials” so they can be entered during your test.
7. Click “Start Test” at the bottom of the pop-up screen to begin your test.

### Running a Test
Once your test begins, the pop-up menu to create a new test will disappear and you will be directed to the test page. From this page you can view various details of your test including:
* which app/website is being tested
* when the test was initiated
* the length of your test
* the status of your test
![Running Test](/images/5_userguide_23.png)

If your test is currently in progress, the status will be denoted as “Running” and a wheel will spin on your screen. Your test will proceed through various stages that can be viewed in the column under “Result”.  
![Stages](/images/5_userguide_24.png)

## Results
Once your test is complete, one of the following results will be displayed:
![Results](/images/5_userguide_25.png)
| Status | Meaning | Display |
|----------|:-------------:|------:|
| Pass | No errors detected during test| ![Pass]|
| Error | 1+ error detected during test| ![Error]|
| Fail | Test failed because of a device failure | ![Fail]|
| Stop | Test ended before the entire scheduled test duration was completed | ![Stop]|
| Cancel | Test was not run due to extended pending time| ![Cancel]|
Click on the device name to open your detailed test report.
![Test Report](/images/5_userguide_26.png)

### App Test Results
#### Summary
The Results Summary page is broken up into 3 sections:
* **Device Info**
This section contains the specifications of the device your test was run on.
* **Test Info**
This section provides information on the test you ran, including the app that was tested and the length of the test.
* **Test Stages**
    This section provides a breakdown of the testing time. You can view the length of each step that occurred during the test.
![App Summary](/images/5_userguide_27.png)

At the bottom of the Summary page, you will see the test result. Any errors found will be documented there, along with an explanation if the test was cancelled, failed, or stopped.

#### Activity Map
![Activity Map](/images/5_userguide_28.png)
The Activity Map shows a flowchart overview of the steps in your test. Each arrow indicates that an action (e.g. clicking a button) lead from one screen to the next.
The navigation tool on the top left allows you to zoom in (+), zoom out (-), and shift the map up/down/left/right.
Along with a flowchart view, you can use the video player controls on the top right to watch a screen recorded video of your test. 
![Controls](/images/5_userguide_29.png)

#### Screen
![Screen](/images/5_userguide_30.png)
The Screen tab allows you to see a detailed screenshot of each step in your test. The blue circle on each screenshot indicates what the testbot clicked. Clicking on a screenshot will open a zoomed in image of each screen. 
Under each screenshot there is a box that gives details including:
* Step number
* Location of action (coordinates)
* Type of action
![Action](/images/5_userguide_31.png)

#### Performance
![Performance](/images/5_userguide_32.png)
The performance tab provides 2 graphs- 1 for CPU Usage and 1 for Memory Usage. 

Dragging your cursor over either graph will show you usage details for a specific time point. If you click on the graph, a pop-up will appear with screenshots and a device log for that time point.

The performance tab is especially helpful if your test contains errors. You can use the graphs to see the exact point where the errors occurred and then click on the timepoint to view screenshots of what caused the error.
![Error](/images/5_userguide_33.png)

####AT&T Video Optimizer
If you choose to run the AT&T Video Optimizer, you will see an AT&T Video Optimizer tab in your result page.
![AT&T Video Optimizer](/images/5_userguide_34.png)

You can download a copy of your ARO Trace File by clicking on “ARO Trace File Download”.1

By clicking on any of the tests under “Test Conducted”, you can see an explanation of the measure and your result.2
![Error](/images/5_userguide_35.png)

#### Log
![Log](/images/5_userguide_36.png)
The Log tab provides you with the full device log for your test. If the device log is multiple pages, use the control bar at the bottom to navigate between different pages.1
Click “Device Full Log Download” on the top right of the page to download the full device log for your test.2 The full log will be saved as a zip file.

### Website Test Results
#### Summary
![Web Summary](/images/5_userguide_37.png)
The Results Summary page provides an overview of completed test.

The Test Info section provides information including the website tested, the length of the test and the browser used for the test.

At the bottom of the Summary page, you will see the overall test result and a breakdown of the types of errors that are found.

#### Activity Map
![Activity Map](/images/5_userguide_38.png)
The Activity Map shows a flowchart overview of the steps in your test. Each arrow indicates that an action (e.g. clicking a button) lead from one screen to the next.
The navigation tool on the top left allows you to zoom in (+), zoom out (-), and shift the map up/down/left/right.
Along with a flowchart view, you can use the video player controls on the top right to watch a screen recorded video of your test. 
![Controls](/images/5_userguide_39.png)

#### Screen
![Screen](/images/5_userguide_40.png)
The Screen tab allows you to see a detailed screenshot of each step in your test. The blue circle on each screenshot indicates what the testbot clicked. Clicking on a screenshot will open a zoomed in image of each screen. 
Under each screenshot there is a box that gives details including:
* Step number
* Location of action (coordinates)
* Type of action
![Action](/images/5_userguide_41.png)

### Share Test Report
Click “Share test report” in the top right corner of the results page if you would like to share your result report.
![Banner](/images/5_userguide_42.png)
If you have any questions while viewing your results and would like help troubleshooting or understanding your results, click “Allow” which will provide apptest.ai administration access to your test results.
If you would like to share your results to others (including those without an apptest.ai account), click “Create” to create a private URL that can be shared. Your report will only be available through this link and the link will be active for 7 days.
![Share](/images/5_userguide_43.png)

### Deleting a Test
Apps and files are automatically removed after 30 days. Test results, screenshots, and logs are stored for 2 months. You can also choose to delete files and results at any time.
You can choose to delete an entire test set from the Projects homepage.
![Delete1](/images/5_userguide_44.png)
You can also delete an individual test from the Test set homepage. 
![Delete2](/images/5_userguide_45.png)
In both cases, press the trash button on the right side of the page. A pop-up will list all the files that will be deleted. Press “Confirm” to delete.
![Delete3](/images/5_userguide_46.png)

## Profile Settings
To access your profile, click your username on the top right of the screen, then select “Profile” from the drop-down list.
![Dash](/images/5_userguide_47.png)

Select the **“Profile”** tab to:
* View your registered email
* Turn on/off email notifications1
* View your registration date
* View your access key
* Change your time zone preferences2
* View length of product usage
![Profile](/images/5_userguide_48.png)

Language settings can be adjusted by clicking on the globe button in the top right corner3. Currently, apptest.ai is available in English and Korean.

Select the **“Teams”** tab to:
* Edit team name1
* Add team members2
* Switch billing manager3
* Turn on/off email notifications4
* Change member privileges (e.g. admin vs. member)5
* Remove team members6 
![Teams](/images/5_userguide_49.png)

Select the **“Change Password”** tab to update your password.
![Change Password](/images/5_userguide_50.png)

Select the **“Billing & Cost”** tab to view your:
* Package Status (Using vs. Booking vs. Expired)
* Package Usage
* Package Order, Start, and Expiration date
![Billing & Cost](/images/5_userguide_51.png)
By clicking on a specific package, you can view further details on your package usage.
![Package Details](/images/5_userguide_52.png)

Select the **“Payment History”** tab to view your detailed payment history including:
* Packages purchased
* Billing Amount
* Payment Method
![Payment History](/images/5_userguide_53.png)

## Further Help
Please consult our [FAQ page](https://apptest.ai/apptest-ai-faq-page/) if you have any questions.
If you need any further help using apptest.ai, please email us at contact@apptest.ai
