---
layout: default-layout
title: iOS User Guide - Dynamsoft MRZ Recognizer
description: This is the user guide page of Dynamsoft MRZ Recognizer for iOS SDK.
keywords: iOS, swift, objective-c, user guide
needAutoGenerateSidebar: true
needGenerateH3Content: true
permalink: /programming/objectivec-swift/user-guide-mrz.html
---

# MRZ Scanner Solution for iOS - User Guide

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
    - [Initialize the MRZ Recognizer](#initialize-the-mrz-recognizer)
    - [Start Recognition Process](#start-recognition-process)
    - [Obtain And Display Recognized MRZ Result](#obtain-and-display-recognized-mrz-result)
    - [Build and Run the Project](#build-and-run-the-project)

## Requirements

- Supported OS: iOS 11.0 or higher.
- Supported ABI: arm64 and x86_64.
- Development Environment: Xcode 13.0 and above (Xcode 14.1+ recommended), CocoaPods 1.11.0+

## Add the Frameworks

The MRZ SCanner Solution iOS Package comes with four frameworks:

- **DynamsoftLabelRecognizer.xcframework**: <a href="https://www.dynamsoft.com/label-recognition/docs/core/introduction/" target="_blank"> Dynamsoft Label Recognizer (DLR) </a> is a framework that offers APIs for text recognition from image files and camera video. **A license is required for its use.**
- **DynamsoftCore.xcframework**: The core framework includes common basic structure and license related APIs.
- **DynamsoftCameraEnhancer.xcframework** (Optional): <a href="https://www.dynamsoft.com/camera-enhancer/docs/core/introduction/" target="_blank"> Dynamsoft Camera Enhancer (DCE) </a> is a framework of getting video frames from mobile cameras. Provides APIs for camera control, camera preview, and other advanced features. **A license is required for its advanced features such as `frame filter`, `sensor control`, `autozoom`, `enhanced focus` and `smart torch`**.
- **MRZScanner.xcframework**: <a href="todo" target="_blank">MRZScanner</a> is a framework wrapped around the Dynamsoft Label Recognizer SDK. It is not a part of the Dynamsoft standard SDK but is completely open source. You can download it and freely modify it.

There are several ways to add the SDK into your project.

### Add the Frameworks Manually

1. Download the solution package from the <a href="todo" target="_blank">Dynamsoft website</a>. After unzipping, four **xcframework** files can be found in the **MRZScanner\Frameworks** directory:

   - **MRZScanner.xcframework**
   - **DynamsoftLabelRecognizer.xcframework**
   - **DynamsoftCore.xcframework**
   - **DynamsoftCameraEnhancer.xcframework**

   > Note:
   > If you want to use iOS Camera SDK or your own sdk to control camera, please ignore **DynamsoftCameraEnhancer.xcframeork** in the following steps.

2. Drag and drop the target frameworks from the above four **xcframework** into your Xcode project. Make sure to check Copy items if needed and Create groups to copy the framework into your project's folder.

3. Click on the project settings then go to **General –> Frameworks, Libraries, and Embedded Content**. Set the **Embed** field to **Embed & Sign** for all of them.

### Add the Frameworks via CocoaPods

1. Add the frameworks in your **Podfile**, replace `TargetName` with your real target name.

   ```pod
   target 'TargetName' do
   use_frameworks!

   pod 'MRZScanner','2.2.20'
   
   # Remove the following line if you want to use iOS AVFoundation framework or your own sdk to control camera.   
   pod 'DynamsoftCameraEnhancer','3.0.1'

   end
   ```

2. Execute the pod command to install the frameworks and generate workspace(**HelloWorld.xcworkspace**):

   ```bash
   pod install
   ```

## Build Your First Application

The following sample will demonstrate how to create a `HelloWorld` app for recognizing MRZ from camera video input. After you complete all steps, the final application looks like this example.

[Download the Objective-C sample source code](todo)
[Download the Swift sample source code](todo)

>Note:
>
>- The following steps are completed in XCode 13.0

### Create a New Project

1. Open XCode and select New Project… in the File > New > New Project… menu to create a new project.

2. Select **iOS -> App** for your application.

3. Input your product name (Helloworld), interface (StoryBoard) and language (Objective-C/Swift)

4. Click on the **Next** button and select the location to save the project.

5. Click on the **Create** button to finish.

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
   >- If the license has expired, you can go to the <a href="https://www.dynamsoft.com/customer/license/trialLicense?utm_source=docs" target="_blank">customer portal</a> to request for an extension.

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

### Obtain And Display Recognized MRZ Result

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
