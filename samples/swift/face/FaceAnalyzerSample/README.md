# Get started with the Azure AI Vision Face Client SDK for iOS (Preview)

In this sample, you will learn how to build and run the face liveness detection application. The Azure AI Vision Face Client SDK for iOS is currently in preview. The APIs are subject to change.

### API Reference Documentation

* Swift API reference documents: [Azure SDK for iOS Docs](https://azure.github.io/azure-sdk-for-ios/), [AzureAIVisionCore](https://azure.github.io/azure-sdk-for-ios/AzureAIVisionCore/index.html), [AzureAIVisionFace](https://azure.github.io/azure-sdk-for-ios/AzureAIVisionFace/index.html)

## Prerequisites

1. An Azure Face API resource subscription.
1. A Mac (with iOS development environment including Xcode 13+ and [Git LFS](https://git-lfs.github.com/)) and an iPhone (with iOS version 14 or above) to test the AzureAIVision SDK.
1. An Apple developer account to install and run development apps on the iPhone.

## Set up the environment 
1. If this is your first time using your Mac to develop, you should build a sample app from [About Me &#x2014; Sample Apps Tutorials | Apple Developer Documentation](https://developer.apple.com/tutorials/sample-apps/aboutme) and run it on your phone before you attempt to build the App here. This will help ensure that your developer environment has been setup properly.
1. If you have a valid Azure subscription that has been provisioned for Face API Liveness Detection, you can get the access token to access the release artifacts. More details can be found in [GET_FACE_ARTIFACTS_ACCESS](../../../../GET_FACE_ARTIFACTS_ACCESS.md).
1. To install the SDK packages for your iOS development Application in Xcode, here are ways through CocoaPods or Swift Package Manager:

   - CocoaPods ([CocoaPods Guides - Getting Started](https://guides.cocoapods.org/using/getting-started.html))
      - Add the following lines to your project's Podfile. `'YourBuildTargetNameHere'` is an example target, and you should use your actual target project instead.

         ```ruby
         # add repos as source
         source 'https://msface.visualstudio.com/SDK/_git/AzureAIVisionCore.podspec'
         source 'https://msface.visualstudio.com/SDK/_git/AzureAIVisionFace.podspec'

         target 'YourBuildTargetNameHere' do
            # add the pods here, optionally with version specification as needed
            pod 'AzureAIVisionCore'
            pod 'AzureAIVisionFace'
         end
         ```

      - For access authorization to the repos, the steps depend on your system Git and your security preferences.
         - If you are using Git credential manager, you will be prompted for username and password. Make a random username and use the accessToken from previous step to be the password. Random username means a random string because it is not really taken in this case.
         - To use [`http.extraHeader` approach of `git-config`](https://git-scm.com/docs/git-config/2.22.0#Documentation/git-config.txt-httpextraHeader), you need to convert the token to base64 format. Refer to [the **Use a PAT** section of this Azure DevOps documentation article](https://learn.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Linux#use-a-pat). Note that instead of using the git clone invocation as shown in the example, you should call:

            ```
            git config --global http.https://msface.visualstudio.com/SDK.extraHeader="Authorization: Basic ${B64_PAT}"
            ```

         - For other types of Git installation, refer to [the **Credentials** section of Git FAQ](https://git-scm.com/docs/gitfaq#_credentials).
   - Swift Package Manager ([Swift.org - Package Manager](https://www.swift.org/documentation/package-manager/))
      - Use the following repository URLs in Swift Package Manager, one at a time:

         ```
         https://msface.visualstudio.com/SDK/_git/AzureAIVisionCore.xcframework
         ```

         ```
         https://msface.visualstudio.com/SDK/_git/AzureAIVisionFace.xcframework
         ````

      - You will see a pop-up window asking for username and password. Make a random username and use the accessToken from previous step to be the password. Random username means a random string because it is not really taken in this case.
      - You may encounter error if your Xcode has never been configured to use Git LFS.
      If Git LFS is never installed on your machine, refer to [Git LFS official site](https://git-lfs.github.com/) for instructions on how to install. To make Xcode recognize the `git-lfs` command, create symbolic link like so:

         ```sh
         sudo ln -s $(which git-lfs) $(xcode-select -p)/usr/bin/git-lfs
         ```

      - If the previous step failed with `ln: git-lfs: Operation not permitted`, your macOS version has [System Integrity Protection (SIP)](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection) enabled and configured to protect the Git executable shipped with Xcode toolchain. There are several options to resolve:
         - Use your security judgment or consult with your enterprise administrators, then temporarily disable SIP before reattempting to run the code above if this is acceptable for you, following instructions in the official article [Disabling and Enabling System Integrity Protection | Apple Developer Documentation](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection).
         - Switch to using your system Git.
            - When you encounter error that says "Package Resolution Failed", dismiss with "Add Anyway". This will add a partially configured package dependency.
            - If Xcode Command Line Tools is never installed on your machine, install it first [following instructions from Apple Developer website](https://developer.apple.com/library/archive/technotes/tn2339/_index.html).
            - Run the following command from Terminal, from the directory where your .xcodeproj is located, as appropriate for your project. It will resolve the package through your system Git. Your system Git should already have Git LFS configured, as mentioned in Prerequisites section.

              ```
              xcodebuild -scmProvider system -resolvePackageDependencies
              ```
       
              > **Note**: You can also add `-project`, `-workspace`, and `-scheme` arguments as appropriate to target your development setup. Refer to the [xcodebuild (Command Line Tools) FAQ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html) and `-help` section.

            - If the command above ran successfully, your project should be able to successfully resolve the dependency from Xcode as well.
         - Alternatively, you may prefer to use CocoaPods instead. This issue has been [reported to Apple](https://github.com/apple/swift-package-manager/issues/5351) and we are awaiting resolution.

1. [Refer to the API reference documentation](#api-reference-documentation) to learn more about our SDK APIs.

## Next steps
Now that you have setup your environment you can either:

- [Build and run sample app](#build-and-run-sample-app) 
- [Integrate face analysis into your own application](#integrate-face-analysis-into-your-own-application)

## Build and run sample App

1. Download the sample App folder. Double click the .xcodeproj file. This will open the project in Xcode.
1. Add package dependency through CocoaPods or Swift Package Manager, as mentioned before. You should add both AzureAIVisionFace.xcframework and AzureAIVisionCore.xcframework into the project.
1. Set the App bundle identifier and developer team in XCode per your needs.
1. Now attach your phone to the Mac. You should get prompt on the phone asking you to "Trust" the Mac. Enable the trust.
1. The phone should now show up in the Xcode top bar. Your iPhone name should be visible.
1. Now build and run the app.

### Run the sample

The first time the app runs, It is going to ask for camera permission. Allow the camera access. The App starts with the launch page. Some of the buttons are disabled until you configure the settings. Click the settings page. The settings page has the following fields/settings from top to bottom. Enter them correctly.

**API endpoint**
This is the Azure subscription endpoint, where the application makes the FaceAPI calls to.

**Subscription**
This secret key to access the Azure endpoint.

### Test out the key scenarios

The application supports 2 scenarios.

#### Liveness
This mode checks to see if the person in camera view is a live person or not.
1. Click on the "Liveness" button, following the guidance on the screen.
2. Click "Start" and show your face to the front-facing camera. As it processes your images, the screen will display user feedback on image quality. The screen will also flash black and white. This is needed for liveness analysis.
3. The result is displayed as Liveness status (Real/Spoof).
4. You can return to the main page by clicking "Continue".

#### LivenessWithVerify
This mode checks the person liveness with verification against a provided face image.
1. Click on the "LivenessWithVerify" button and it will prompt you to select an image of a face to verify against.
2. Click next and show your face to the front-facing camera. As it processes your images, the screen will display user feedback on image quality. The screen will also flash black and white. This is needed for liveness analysis.
3. The result is displayed as Liveness status (Real/Spoof), verification status (Recognized/NotRecognized), and verification confidence score.
4. You can return to the main page by clicking "Continue".

## Integrate face analysis into your own application
Based on the provided sample App, here is the instructions on how to integrate face analysis function into your own App. First, face analysis using AzureAIVision SDK requires a Vision source and a Vision service to be configured. The Vision source for the mobile scenario defaults to built in camera on the mobile device. The Vision source is wrapped into a SwiftUI View object that is required for a preview (please refer to sample App file "CameraPreviewView.swift"). The Vision service requires Azure subscription. The Azure subscription prerequisites are also common to all scenarios.
In the sample App, we provide sample views like: MainView.swift, LaunchView.swift, ResultView.swift, SettingsView.swift to help with the App flow. These files are not needed in your Application, and please take them for your reference only. More importantly, we provide the following example code to interact with the camera and the AzureAIVision SDK, which you should adapt into your own App properly. For your convenience, we put all those required files under the "FaceAnalyerSample/Core" folder.

Here are the recommended steps you should consider to follow during your integration. Also, here is an companion video that shows **[how to do the integration in an empty Xcode project](https://aka.ms/azure-ai-vision-face-liveness-client-sdk-ios-integration-video)**.

[![iOS integration video front](../../../../docs/face/iOS-integration-video-front.jpg)](https://aka.ms/azure-ai-vision-face-liveness-client-sdk-ios-integration-video)

### 1. Configure your Xcode project

   (1) in the "Xcode -> Targets -> Build Settings -> Swift Compiler - Language", select the "C++ and Objective-C Interoperability" to be "C++ / Objective-C++

   (2) in the "Xcode -> Targets -> Info -> Custom iOS Target Properties", add Key "Privacy-Camera Usage Description" with your description, like "This App requires camera usage."

### 2. Add package dependency

   In the "Xcode -> Files -> Add Package Dependencies", add the AzureAIVisionCore.xcframework, AzureAIVisionFace.xcframework as mentioned in [Set up the environment](#set-up-the-environment).

### 3. Copy the required files

   Copy all the files under FaceAnalyzerSample/Core folder into your project.

### 4. Insert LivenessView into your UI logic

   "LivenessView" is the view you should insert into your application to serve for the liveness detection.
   Here are more details about the LivenessView init parameters.

   For more information on how to orchestrate the liveness flow by utilizing the Azure AI Vision Face service, visit: https://aka.ms/azure-ai-vision-face-liveness-tutorial.

   (1) sessionAuthorizationToken: String.

   required parameter. Used to authorize the client and allow the client to establish the session connection to the server.

   (2) withVerification: Boolean.

   Optional parameter, default as false. This boolean indicates whether the liveness detection is with verification or not.

   (3) referenceImage: UIImage?.

   Optional parameter, default as nil. This refers to the reference image provided in client. You can also provide this in the App server when creating the session.

   (4) completionHandler: @escaping (String, String)->Void.

   Optional parameter, default as empty function. Used to handle the liveness detection results and also help on the UI switch possibly, as shown in the FaceAnalyzerSample/MainView.swift, the completionHandler is set as:
   ```
   completionHandler: { resultMessage, resultId in
                        sessionData.resultMessage = resultMessage
                        sessionData.resultId = resultId
                        DispatchQueue.main.async {
                           withAnimation {
                                 pageSelection.current = .result
                           }
                        }
                     }
   ```
   In this case, the liveness result is processed to be a string "resultMessage" and saved in the sample App's "sessionData" object. The "resultId" represents the client requestId for trace logs purpose. You can modify the "completionHandler" definition and functionality here to process the liveness result in your own way and may also utilize it to help for your UI logic.

   (5) detailsHandler: @escaping (FaceAnalyzedDetails?)->Void.

   Optional parameter, default as empty function. Used to handle the "digest". You can get the "digest" like:
   ```
   var digest = faceAnalyzedDetails?.digest
   ```
   This "digest" is recommended to be used to validate the integrity of the transport.
   For more information, please refer to the later part: [Add validation for the integrity of the service result](#7-add-validation-for-the-integrity-of-the-service-result).

   Create other views per your needs, like what we have in the sample App (MainView.swift, LaunchView.swift, etc) to start/end the process, and save the face analysis results for your usage.

### 5. Configure the liveness algorithm details for advanced settings

   With first 4 steps, you should be able to run liveness detection in your own project. Here are more advanced details for you to understand the API usage. The file LivenessActor.swift contains the method on how to create and initialize the "FaceAnalyzer". Specifically,

   (1) ***Configuring the FaceAPI service to obtain the required session-authorization-token***
   ```swift
   // this is for demo purpose only, session-authorization-token can be obtained in the App server directly
   let sessionAuthorizationToken = obtainToken(...)
   serviceOptions = try VisionServiceOptions()
   serviceOptions.authorizationToken = sessionAuthorizationToken
   ```

   Note:
   * A demo version on obtaining the token is in `AppUtility.swift` for the demo app to be built as an standalone solution, but this is not recommended.  The "session-authorization-token" is required to start a liveness session.  For more information on how to orchestrate the liveness flow by utilizing the Azure AI Vision Face service, visit: https://aka.ms/azure-ai-vision-face-liveness-tutorial.

   (2) ***Configuring the face analyzer***
   ```swift
   let createOptions = try! FaceAnalyzerCreateOptions()
   createOptions.faceAnalyzerMode = FaceAnalyzerMode.trackFacesAcrossImageStream
   ```
   (3)  ***Configuring the Analysis method***
   ```swift
   let methodOptions = try! FaceAnalysisOptions()
   methodOptions.faceSelectionMode = FaceSelectionMode.largest
   ```
   (4)  ***Initializing the face analyzer***
   ```swift
   faceAnalyzer = try await FaceAnalyzer.create(serviceOptions: serviceOptions, input: visionSource, createOptions: createOptions)
   ```
   (5)  ***Registering listeners for analyzer events and results***
   ```swift
   faceAnalyzer?.addAnalyzingEventHandler{(analyzer: FaceAnalyzer, result: FaceAnalyzingResult) in
       let faces = result.faces
       let face = faces?[faces!.startIndex]
       // your analyzingEventHandler
   }

   faceAnalyzer?.addAnalyzedEventHandler {[parameters] (analyzer: FaceAnalyzer, result: FaceAnalyzedResult) in
       let faces = result.faces
       let face = faces?[faces!.startIndex]
       // your analyzedEventHandler
   }
   ```
   (6)  ***Starting the analyzer***
   ```swift
   faceAnalyzer?.analyzeOnce(using: methodOptions, completionHandler: { (result, error) in
       // your analyzeOnce completionHandler
   })
   ```

### 6. Add required localizations

   All the on-screen prompt are defined with English as default language. We provide Chinese (simplified) as an example to add your localization.

   (1) Go to "Xcode -> Targets -> Info -> Custom iOS Target Properties -> Localizations" to add all the languages you want to support.

   (2) Refer to the Core/en.lproj and Core/zh-Hans.lproj to add the corresponding translation for all the added languages in your localizations.

### 7. Add validation for the integrity of the service result

   We highly recommend leveraging the "digest" generated within the solution to validate the integrity of the communication between your application and the Azure AI Vision Face service. This is necessary to ensure that the final liveness detection result is trustworthy. "Digest" is provided in the following two locations:
1. The Face Analyzer running on your application.

   In the FaceAnalyzedDetails object.
2. The Azure AI Vision Face service.

   The "digest" will be contained within the liveness detection result when calling the detectliveness/singlemodal/sessions/<session-id> REST call. Look for an example of the "digest" in the [tutorial](https://aka.ms/azure-ai-vision-face-liveness-tutorial) where the liveness detection result is shown.

   Digests must match between the application and the service. We recommend using these digests in conjunction with iOS integrity APIs to perform the final validation.
   For more information on the iOS Integrity APIs, please refer to:
   - [DeviceCheck | Apple Developer Documentation](https://developer.apple.com/documentation/devicecheck)

