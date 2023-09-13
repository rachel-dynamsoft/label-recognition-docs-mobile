---
layout: default-layout
title: iOS User Guide - Dynamsoft MRZ Recognizer
description: This is the user guide page of Dynamsoft MRZ Recognizer for iOS SDK.
keywords: iOS, swift, objective-c, user guide
needAutoGenerateSidebar: true
needGenerateH3Content: true
permalink: /programming/objectivec-swift/user-guide-mrz.html
---

# Dynamsoft MRZ Scanner for iOS Package - User Guide

- [MRZ Scanner Solution for iOS - User Guide](#mrz-scanner-solution-for-ios---user-guide)
  - [Requirements](#requirements)
  - [Add the Frameworks](#add-the-frameworks)
    - [Add the Frameworks Manually](#add-the-frameworks-manually)
    - [Add the Frameworks via CocoaPods](#add-the-frameworks-via-cocoapods)
  - [Build Your First Application](#build-your-first-application)
    - [Create a New Project](#create-a-new-project)
    - [Include the Frameworks](#include-the-frameworks)
    - [Initialize the License](#initialize-the-license)
    - [Initialize the Camera Module](#initialize-the-camera-module)
    - [Initialize the MRZ Scanner](#initialize-the-mrz-recognizer)
    - [Start MRZ Scanning](#start-recognition-process)
    - [Obtain and Display Recognized MRZ Result](#obtain-and-display-recognized-mrz-result)
    - [Build and Run the Project](#build-and-run-the-project)

## Requirements

- **Supported OS**: iOS 11.0 or higher.
- **Supported ABI**: arm64 and x86_64.
- **Development Environment**: Xcode 13.0 and above (Xcode 14.1+ recommended), CocoaPods 1.11.0+

## Add the Frameworks

The MRZ Scanner for iOS Package comes with four frameworks:

- **DynamsoftCore.xcframework**: The core framework includes common basic structure and license related APIs.
- **DynamsoftLabelRecognizer.xcframework**: <a href="https://www.dynamsoft.com/label-recognition/docs/core/introduction/" target="_blank"> Dynamsoft Label Recognizer (DLR) </a> is a framework that offers APIs for text recognition from image files and camera video. **A license is required for its use.**
- **MRZScanner.xcframework**: <a href="todo" target="_blank">MRZScanner</a> is a framework wrapped around the Dynamsoft Label Recognizer SDK for MRZ scanning. 
- **DynamsoftCameraEnhancer.xcframework** (Optional): <a href="https://www.dynamsoft.com/camera-enhancer/docs/core/introduction/" target="_blank"> Dynamsoft Camera Enhancer (DCE) </a> is a framework for getting video frames from mobile cameras. DCE provides APIs for camera control, camera preview, and more. A license is required for its advanced features such as **frame filter**, **sensor control**, **auto zoom**, **enhanced focus** and **smart torch**. 

  > Note: If you want to use iOS Camera SDK or your own sdk to control camera, please ignore **DynamsoftCameraEnhancer.xcframeork** in the following steps.

You can add the MRZ scanner frameworks to your project manually or via CocoaPods.

### Add the Frameworks Manually

To add the MRZ scanner frameworks manually, please follow these steps:

1. Download the solution package from the <a href="todo" target="_blank">Dynamsoft website</a>. After unzipping, four `xcframework` files can be found in the `MRZScanner\Frameworks` directory:

   - **MRZScanner.xcframework**
   - **DynamsoftLabelRecognizer.xcframework**
   - **DynamsoftCore.xcframework**
   - **DynamsoftCameraEnhancer.xcframework**

2. Drag and drop the desired XCFrameworks into your project. Make sure to select **Copy items if needed** and **Create groups** to copy the framework into your project's folder.

3. In Xcode, navigate to your project settings and select **General** from the top menu.

4. Scroll down to the **Frameworks, Libraries, and Embedded Content** section and ensure that the XCFrameworks you added in step 2 are listed.

5. For each XCFramework, set the **Embed** field to **Embed & Sign**. This will ensure that the frameworks are embedded in your app and properly signed.

### Add the Frameworks via CocoaPods

To add the MRZ scanner frameworks via CocoaPods, please follow these steps:

1. Add the frameworks in your `Podfile`, replace `TargetName` with your real target name.

   ```pod
   target 'TargetName' do
   use_frameworks!

   pod 'MRZScanner','2.2.20'
   
   # Remove the following line if you want to use iOS AVFoundation framework or your own sdk to control camera.   
   pod 'DynamsoftCameraEnhancer','3.0.1'

   end
   ```

2. Execute the pod command to install the frameworks and generate `HelloWorld.xcworkspace`:

   ```bash
   pod install
   ```

## Build Your First Application

The following sample will demonstrate how to create a HelloWorld app for recognizing MRZ from camera video input. After you complete all the steps, the final application will look like the example provided.

Download the source code for either Objective-C or Swift:

- [Download MRZ Scan sample code [Objective-C]](todo)
- [Download MRZ Scan sample code [Swift]](todo)

>Note:
>- These steps were completed in XCode 13.0

### Create a New Project

To create a new project in Xcode, follow these steps:

1. Open Xcode and navigate to **File > New > New Project…**

2. Select **App** under the **iOS** category.

3. Choose a product name (e.g. Helloworld), interface (Storyboard), and language (Objective-C or Swift).

4. Click **Next** and select a location to save your project.

5. Click **Create** to finish.

### Include the Frameworks

Add the SDK to your new project. Please read [Add the Frameworks](#add-the-frameworks) section for more details.

### Initialize the License

1. Use the `DynamsoftLicenseManager` class and initialize the license in the file **AppDelegate**.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   @interface AppDelegate ()<LicenseVerificationListener>
   ...
   - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      ...
      [DynamsoftLicenseManager initLicense:@"DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9" verificationDelegate:self];
   }
   - (void)licenseVerificationCallback:(bool)isSuccess error:(NSError *)error {
      // Add code to execute when license verification is approved or failed.
   }
   ```
   2. 
   ```swift
   class AppDelegate: UIResponder, UIApplicationDelegate, LicenseVerificationListener {
      func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
             DynamsoftLicenseManager.initLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9",verificationDelegate:self)
      }
      func licenseVerificationCallback(_ isSuccess: Bool, error: Error?) {
             // Add code to execute when license verification is approved or failed.
      }
   }
   ```

   >Note:  
   >  
   >- Network connection is required for the license to work.
   >- The license string here will grant you a time-limited trial license.
   >- If the license has expired, you can log into the <a href="https://www.dynamsoft.com/customer/license/trialLicense?utm_source=docs" target="_blank">Dynamsoft customer portal</a> to request for an extension.

### Initialize the Camera Module

1. Go to the file **ViewController**, create the instances of `DynamsoftCameraEnhancer` and `DCECameraView`.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   @interface ViewController ()
   @property (nonatomic, strong) DynamsoftCameraEnhancer *cameraEnhancer;
   @property (nonatomic, strong) DCECameraView *dceView;
   ...
   @end
   @implementation ViewController
   - (void)viewDidLoad {
      [super viewDidLoad];
      self.view.backgroundColor = [UIColor whiteColor];
      self.title = @"MRZ Scanner";
      [self configureMRZ];
   }
   - (void)configureMRZ {
      self.dceView = [[DCECameraView alloc] initWithFrame:self.view.bounds];
      self.cameraEnhancer = [[DynamsoftCameraEnhancer alloc] initWithView:self.dceView];
      [self.view addSubview:self.dceView];
      [self.cameraEnhancer open];
   }
   @end
   ```
   2. 
   ```swift
   class ViewController: BaseViewController{
      var cameraEnhancer: DynamsoftCameraEnhancer!
      var dceView: DCECameraView!
      ...
      func configureMRZ() -> Void {
             dceView = DCECameraView.init(frame: self.view.bounds)
             cameraEnhancer = DynamsoftCameraEnhancer.init(view: self.dceView)
             self.view.addSubview(self.dceView)
             cameraEnhancer.open()
      }
   }
   ```

2. Define a scan region for MRZ recognition.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   - (void)configureMRZ {
      ...
      iRegionDefinition *region = [[iRegionDefinition alloc] init];
      region.regionLeft = 5;
      region.regionRight = 95;
      region.regionTop = 40;
      region.regionBottom = 60;
      region.regionMeasuredByPercentage = 1;
      [self.cameraEnhancer setScanRegion:region error:nil];
   }
   ```
   2. 
   ```swift
   func configureMRZ() -> Void {
      ...
      let region = iRegionDefinition.init()
      region.regionLeft = 5
      region.regionRight = 95
      region.regionTop = 40
      region.regionBottom = 60
      region.regionMeasuredByPercentage = 1
      try? cameraEnhancer.setScanRegion(region)
   }
   ```

### Initialize the MRZ Recognizer

1. Create an instance of `MRZRecognizer`, bind it with the instance of `DynamsoftCameraEnhancer`.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   @interface ViewController ()
   ...
   @property (nonatomic, strong) DynamsoftMRZRecognizer *mrzRecognizer;
   @end
   @implementation ViewController
   - (void)configureMRZ {
      self.mrzRecognizer = [[DynamsoftMRZRecognizer alloc] init];
      [self.mrzRecognizer setImageSource:self.cameraEnhancer];
   }
   @end
   ```
   2. 
   ```swift
   class ViewController: BaseViewController{
      var mrzRecognizer: DynamsoftMRZRecognizer!
      ...
      func configureMRZ() -> Void {
         mrzRecognizer = DynamsoftMRZRecognizer.init()
         mrzRecognizer.setImageSource(self.cameraEnhancer)
      }
   }
   ```

### Start Recognition Process

1. Start the MRZ recognition process and implement the `MRZResultListener` protocol to retrieve the recognized MRZ result.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   @interface ViewController ()<MRZResultListener>
   ...
   @end
   @implementation ViewController
   - (void)configureMRZ {
      [self.mrzRecognizer setMRZResultListener:self];
      [self.mrzRecognizer startScanning];
   }
   - (void)mrzResultCallback:(NSInteger)frameId imageData:(iImageData *)imageData result:(iMRZResult *)result {
      // Add your code to execute on results are received.
   }
   @end
   ```
   2. 
   ```swift
   class ViewController: BaseViewController, MRZResultListener {
      ...
      func configureMRZ() -> Void {
         ...
         mrzRecognizer.setMRZResultListener(self)
         mrzRecognizer.startScanning()
      }
      func mrzResultCallback(_ frameId: Int, imageData: iImageData, result: iMRZResult?) {
         // Add your code to execute on results are received.
      }
   }
   ```

### Obtain and Display Recognized MRZ Result

1. Obtain the MRZ result via `mrzResultCallback` and display the it on the `UIAlertController`.

   <div class="sample-code-prefix"></div>
   >- Objective-C
   >- Swift
   >
   >1. 
   ```objc
   @interface ViewController ()<MRZResultListener>
   ...
   @end
   @implementation ViewController
   ...
   - (void)mrzResultCallback:(NSInteger)frameId imageData:(iImageData *)imageData result:(iMRZResult *)result {
    if (result != nil) {
        [self.mrzRecognizer stopScanning];
        [self showResult:result];
      }
   }
   - (void)showResult:(iMRZResult*)result {
      NSString *isParsedString = (result.isParsed) ? @"YES" : @"NO";
      NSString *isVerifiedString = (result.isVerified) ? @"YES" : @"NO";
      //combine the mrz result into a string.
      NSString *resultString = [NSString stringWithFormat:@"Document Type:%@\nIssuing State:%@\nSurname:%@\nGiven Name:%@\nDocument Id:%@\nNationlity:%@\nDate of Birth(YY-MM-DD):%@\nGender:%@\nDate of Expiry(YY-MM-DD):%@\nIs Parsed:%@\nIs Verified:%@\nMRZ Text:%@", result.docType, result.issuer, result.surname, result.givenName, result.docId, result.nationality, result.dateOfBirth, result.gender, result.dateOfExpiration, isParsedString, isVerifiedString, result.mrzText];
      //display the result
      UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"Result" message:resultString preferredStyle:UIAlertControllerStyleAlert];
      UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
         [self.mrzRecognizer startScanning];
      }];
      [alertController addAction:okAction];
      [self presentViewController:alertController animated:YES completion:nil];
   }
   @end
   ```
   1. 
   ```swift
   class ViewController: BaseViewController, MRZResultListener {
      ...
      func mrzResultCallback(_ frameId: Int, imageData: iImageData, result: iMRZResult?) {
         if let result = result {
               mrzRecognizer?.stopScanning()
               showResult(result)
         }
      }
      func showResult(_ result: iMRZResult) {
         let isParsedString = result.isParsed ? "YES" : "NO"
         let isVerifiedString = result.isVerified ? "YES" : "NO"
         let resultString = """
               Document Type: \(result.docType)
               Issuing State: \(result.issuer)
               Surname: \(result.surname)
               Given Name: \(result.givenName)
               Document Id: \(result.docId)
               Nationlity: \(result.nationality)
               Date of Birth(YY-MM-DD): \(result.dateOfBirth)
               Gender: \(result.gender)
               Date of Expiry(YY-MM-DD): \(result.dateOfExpiration)
               Is Parsed: \(isParsedString)
               Is Verified: \(isVerifiedString)
               MRZ Text: \(result.mrzText)
               """
         let alertController = UIAlertController(title: "Result", message: resultString, preferredStyle: .alert)
         let okAction = UIAlertAction(title: "OK", style: .default) { [weak self] (_) in
               self?.mrzRecognizer?.startScanning()
         }
         alertController.addAction(okAction)
         present(alertController, animated: true, completion: nil)
      }
   }
   ```

### Build and Run the Project

1. Select the device that you want to run your app on.
2. Run the project, then your app will be installed on your device.
