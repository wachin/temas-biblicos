---
title: IBM MobileFirst Platform Foundation Support for iOS 12
date: 2018-07-23
tags:
- MobileFirst_Platform
- Announcement
- iOS
version:
- 8.0
- 7.1
author:
  name: Sandhya Suman
---

> **Update:** Please refer our latest [blog]({{site.baseurl}}/blog/2018/09/17/IBM-MobileFirst-iOS12/) for the latest news on iOS12 compatibility with MobileFirst .


If you are an on-premise 7.1 or 8.0 customer or [Mobile Foundation Service](https://console.bluemix.net/catalog/services/mobile-foundation) customer, then read further to learn about  Mobile Foundation support for iOS 12 .

Apple announced iOS 12 during their annual *WWDC2018* conference. This release of iOS includes many new user and developer oriented features. You can read more about all the new features in iOS 12 [here](https://developer.apple.com/ios/whats-new/).

We have been testing the iOS 12 beta with the latest being beta 3. We have verified various features of MobileFirst Platform Foundation on the iOS 12 beta 3 for MobileFirst Platform Foundation v7.1 and v8.0. For reference, make sure to read through our [support plan for newer iOS version](https://mobilefirstplatform.ibmcloud.com/blog/2017/01/11/support-plan-for-next-android-ios-mobile-os/).

We encourage you to start testing your application(s) with iOS 12.

>**Update:** *If you are planning to build your MobileFirst v8 application for iOS12  you have to remove stdc++ library from your Dependencies. This is because stdc++ is removed in iOS 12. Refer Known Issues section in this blog for details of the workaround.*
>
>*For Cordova apps, please upgrade to the latest iFix [mfp-ifix-IF201809041150]({{site.baseurl}}/blog/2018/05/18/8-0-master-ifix-release/#collapse-mfp-ifix-IF201809041150) so that your Cordova projects have the correct reference of libc++ automatically. For v7.1 , you can upgrade to the iFix [ 7.1.0.0-MFPF-IF201808161052]({{site.baseurl}}/blog/2018/05/18/7-1-master-ifix-release/).*

## MobileFirst Platform Foundation Support for iOS 12(beta 3)

#### Existing application
Existing application(s) that were created using MobileFirst Platform v7.1 and v8.0 **will work** on iOS 12 as they did on previous versions of iOS.

#### Updating application on the App Store (built using older Xcode)
You can opt to build application(s) with Xcode 9 and republish to the App Store. These application(s) will work on iOS 12. Make sure to build app with only 64 bit architecture. Apple [mandates](https://developer.apple.com/app-store/submissions) that iOS apps and app updates submitted to the App Store from must be built with the Xcode 9 and iOS 11 SDK and must support the Super Retina display of iPhone X. Ensure that your apps are built to meet these requirements.

#### Updating existing application or submitting new application on the App Store (built using Xcode 10)
Review the following section to learn what actions you need to take so that your app can support iOS 12. These needs to be considered only if you are building the application(s) using new [Xcode 10 build](https://developer.apple.com/download).

Compatibility tests with [iOS 12 beta 3](https://developer.apple.com/download) have been performed using an iPhone 6 plus that was upgraded from iOS 11.3.x to iOS 12 beta 3 for MobileFirst Foundation v8.0 and on iOS12 beta 6 for MobileFirst Foundation v7.1.

For the tests, [XCode 10 beta 2](https://developer.apple.com/download) was used to compile native, hybrid and Cordova apps. Xcode 10 uses a new build system. The new build system provides improved reliability and build performance and it catches project configuration problems that the legacy build system could not.
The legacy build system is still available in Xcode 10. To use the legacy build system, select it in the **File > Project Settings**. Refer to the image below:

![Xcode10 Legacy build system settings]({{site.baseurl}}/assets/blog/2017-07-20-compatibility-tests-for-ios-12/xcode10-buildsystem.png)

For the compatibility test we had to use "legacy build system" to get rid of the issue reported [here](https://stackoverflow.com/questions/50718018/xcode-10-error-multiple-commands-produce)
For more details on Xcode 10, refer [Whats new in XCode10](https://developer.apple.com/xcode/whats-new/).

>**Update:** *Lately we verified MobileFirst features with Xcode 10 with new build system without any issues.*


#### Swift Apps on iOS12
Along with iOS 12 apple introduced **Swift 4.2** for Swift developers. [Xcode 10 build](https://developer.apple.com/download) can build targets written in only Swift 4.x or Swift 3.2. Xcode 10 can be used for migration to Swift 4, This can be easily done using the [migration guide](https://swift.org/migration-guide). Apple introduced  **Swift 4.2** to be an intermediate step towards achieving ABI stability in **Swift 5**, which should enable binary compatibility between applications and libraries compiled with different Swift versions.

As per Apple you can submit apps in **Swift 3.2** to the App Store and migrate individual modules to Swift 4 when you are ready. We strongly encourage you to migrate your code to **Swift 4.2** in order to prepare for the ABI stability coming in **Swift 5.0**.

#### Deprecation of UIWebView
Apple also announced the deprecation of **UIWebView** whose replacement is the **WKWebView**. This does not have any impact on MobileFirst features in iOS 12. MobileFirst v8.0 already supports **WKWebview** using the **WKWebview** Cordova plugin (). We will be migrating to **WKWebview** in the MobileFirst SDK in a future iFix.

Highlighted features that were tested for Mobilefirst Foundation v7.1 and v8.0:

* OAuth Flow
* Invoking backend procedures through adapters
* Challenge Handling
* JSONStore
* Direct Update
* Application Management
* Application Authenticity
* Remote Disable
* Device SSO
* Push notifications

We had also verified MFP based cordova, ionic and react-native apps.

All the above features were validated to work on iOS 12 beta 3.  

### Known Issues
Starting with iOS 12, **stdc++** library is removed from simulator runtime, but it remains in the iOS 12.0 (device) runtime for binary compatibility with shipping apps. **libstdc++** was deprecated 5 years ago. Apple's more recent platforms (tvOS and watchOS) doesn't support it.
In order to test your app on simulator, please use the workaround suggested [here](https://stackoverflow.com/questions/50694822/xcode-10-ios-12-does-not-contain-libstdc6-0-9).
Mostly, for a hybrid/cordova app we found that entry of **libstdc++** included in our project structure  was red because Xcode couldn't find the reference for it. After we removed the entry of **libstdc++** from the project structure and added the new lib instead(**libc++**), it worked without issues.
For a native app , you should remove **libstdc++** from other linker flags in your xcode project as below:

  ![iOS12 libstdc++  issue workaround]({{site.baseurl}}/assets/blog/2017-07-20-compatibility-tests-for-ios-12/ios12-stdlib-fix.png)

* During our compatibility test we found certificate pinning feature is not working. It looks very similar to the issue mentioned [here](https://github.com/AFNetworking/AFNetworking/issues/4229). We are currently investigating the issue. We will update here once we conclude our investigation.

>**Update:** *We verified Certificate Pinning with latest beta and found, certificate pinning is working fine. Previously, evaluating trust was failing due to the issue Certificate.*

iOS 12 compatibility tests are planned for following:

* Application Center
* watchOS

*Stay tuned for more updates.*

> **Disclaimer:** *Some of the action items that are addressed in the list above are not under IBM’s control. Therefore, we expect developers and IT managers to ensure that their infrastructure is up-to-date according to Apple’s requirements.*
