---
layout: default-layout
title: Android User Guide - Dynamsoft MRZ Recognizer
description: This is the user guide page of Dynamsoft MRZ Recognizer for Android Language.
keywords: a, user guide
needAutoGenerateSidebar: true
needGenerateH3Content: true
permalink: /programming/android/user-guide-mrz.html
---

# MRZ Scanner Solution for Android - User Guide

- [MRZ Scanner Solution for Android - User Guide](#mrz-scanner-solution-for-android---user-guide)
  - [Requirements](#requirements)
  - [Add the Libraries](#add-the-libraries)
    - [Add the Libraries Manually](#add-the-libraries-manually)
    - [Add the Libraries via Maven](#add-the-libraries-via-maven)
  - [Build Your First Application](#build-your-first-application)
    - [Create a New Project](#create-a-new-project)
    - [Include the Libraries](#include-the-libraries)
    - [Initialize the License](#initialize-the-license)
    - [Initialize Camera Module](#initialize-camera-module)
    - [Initialize MRZ Recognizer](#initialize-mrz-recognizer)
    - [Start Recognition Process](#start-recognition-process)
    - [Obtain and Display Recognized MRZ Result](#obtain-and-display-recognized-mrz-result)
    - [Build and Run the Project](#build-and-run-the-project)

## Requirements

- Supported OS: Android 5.0 (API Level 21) or higher.
- Supported ABI: **armeabi-v7a**, **arm64-v8a**, **x86** and **x86_64**.
- Development Environment: Android Studio 3.4+ (Android Studio 4.2+ recommended).

## Add the Libraries

The MRZ SCanner Solution Android Package comes with four libraries:

- **DynamsoftLabelRecognizer.aar**: <a href="https://www.dynamsoft.com/label-recognition/docs/core/introduction/" target="_blank"> Dynamsoft Label Recognizer (DLR) </a> is a library that offers APIs for text recognition from image files and camera video. **A license is required for its use.**
- **DynamsoftCore.aar**: The core library includes common basic structure and license related APIs.
- **DynamsoftCameraEnhancer.aar** (Optional): <a href="https://www.dynamsoft.com/camera-enhancer/docs/core/introduction/" target="_blank"> Dynamsoft Camera Enhancer (DCE) </a> is a library of getting video frames from mobile cameras. Provides APIs for camera control, camera preview, and other advanced features. **A license is required for its advanced features such as `frame filter`, `sensor control`, `autozoom`, `enhanced focus` and `smart torch`**.
- **MRZScanner.aar**: <a href="todo" target="_blank">MRZScanner</a> is a library wrapped around the Dynamsoft Label Recognizer SDK. It is not a part of the Dynamsoft standard SDK but is completely open source. You can download it and freely modify it.

There are two ways to add the libraries into your project - **Manually** and **Maven**.

### Add the Libraries Manually

1. Download the solution package from the <a href="todo" target="_blank">Dynamsoft Website</a>. After unzipping, four **aar** files can be found in the **MRZScanner\Libs** directory:

   - **MRZScanner.aar**
   - **DynamsoftLabelRecognizer.aar**
   - **DynamsoftCore.aar**
   - **DynamsoftCameraEnhancer.aar** (Optional)
      >Note:
      >
      >If you want to use Android Camera SDK or your own sdk to control camera, please ignore **DynamsoftCameraEnhancer.aar** in the following steps.

2. Copy the above two **aar** files to the target directory such as `[App Project Root Path]\app\libs`

3. Open the file `[App Project Root Path]\app\build.gradle` and add reference in the dependencies:

    ```groovy
    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.aar'])

        def camerax_version = '1.1.0'
        implementation "androidx.camera:camera-core:$camerax_version"
        implementation "androidx.camera:camera-camera2:$camerax_version"
        implementation "androidx.camera:camera-lifecycle:$camerax_version"
        implementation "androidx.camera:camera-view:$camerax_version"
    }
    ```

    > Note:
    >
    > DCE 3.x is based on Android CameraX, so you need to add the CameraX dependency manually.

4. Click **Sync Now**. After the synchronization completes, the SDK is added to the project.

### Add the Libraries via Maven

1. Open the file `[App Project Root Path]\app\build.gradle` and add the maven repository:

    ```groovy
    repositories {
         maven {
            url "https://download2.dynamsoft.com/maven/aar"
         }
    }
    ```

2. Add reference in the dependencies:

   ```groovy
   dependencies {
      implementation 'com.dynamsoft:mrzscanner:2.2.20'
      // Remove the following line if you want to use Android Camera sdk or your own sdk to control camera.
      implementation 'com.dynamsoft:dynamsoftcameraenhancer:3.0.1'
   }
   ```

3. Click **Sync Now**. After the synchronization completes, the SDK is added to the project.

## Build Your First Application

The following sample will demonstrate how to create a `HelloWorld` app for recognizing MRZ from camera video input. After you complete all steps, the final application looks like this example.

[Download the sample source code](todo)

>Note:
>
>- The following steps are completed in Android Studio 4.2.

### Create a New Project

1. Open Android Studio and select `New Project…` in the `File > New > New Project…` menu to create a new project.

2. Choose the correct template for your project. In this sample, we'll use `Empty Activity`.

3. When prompted, choose your app name (`HelloWorld`) and set the Save location, Language, and Minimum SDK (21)
    >Note: With minSdkVersion set to 21, your app is available on more than 94.1% of devices on the Google Play Store (last update: March 2021).

### Include the Libraries

Add the SDK to your new project. Please read [Add the Libraries](#add-the-libraries) section for more details.

### Initialize the License

1. Import the `LicenseManager` class and initialize the license in the file `MainActivity.java`.

   ```java
   import com.dynamsoft.core.LicenseManager;
   import com.dynamsoft.core.LicenseVerificationListener;
   import com.dynamsoft.core.CoreException;

   public class MainActivity extends AppCompatActivity {

      @Override
      protected void onCreate(Bundle savedInstanceState) { 
         super.onCreate(savedInstanceState);
         setContentView(R.layout.activity_main);

         LicenseManager.initLicense("DLS2eyJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSJ9", MainActivity.this, new LicenseVerificationListener() {
            @Override
            public void licenseVerificationCallback(boolean isSuccess, CoreException error) {
               if(!isSuccess){
                  error.printStackTrace();
               }
            }
         });
      }
   }
   ```

   >Note:  
   >  
   >- Network connection is required for the license to work.
   >- The license string here will grant you a time-limited trial license.
   >- If the license has expired, you can go to the <a href="https://www.dynamsoft.com/customer/license/trialLicense?utm_source=docs" target="_blank">customer portal</a> to request for an extension.

### Initialize Camera Module

1. In the Project window, open **app > res > layout > `activity_main.xml`** and create a DCE camera view section under the root node.

    ```xml
    <com.dynamsoft.dce.DCECameraView
        android:id="@+id/camera_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent">
    </com.dynamsoft.dce.DCECameraView>
    ```

2. Import the dynamsoft camera module, initialize a `DCECameraView` and bind to the created `CameraEnhancer` instance in the file `MainActivity.java`.

   ```java
   ...
   
   import com.dynamsoft.dce.DCECameraView;
   import com.dynamsoft.dce.CameraEnhancer;
   import com.dynamsoft.dce.CameraEnhancerException;

   public class MainActivity extends AppCompatActivity {
      private CameraEnhancer mCamera;

      @Override
      protected void onCreate(Bundle savedInstanceState) { 

         ...

         DCECameraView cameraView = findViewById(R.id.camera_view);

         mCamera = new CameraEnhancer(MainActivity.this);
         mCamera.setCameraView(cameraView);
      }
   }
   ```

3. Define a scan region for MRZ recognition.

   ```java
   ...
   
   import com.dynamsoft.core.RegionDefiniton;

   public class MainActivity extends AppCompatActivity {

      @Override
      protected void onCreate(Bundle savedInstanceState) { 

        ...

        RegionDefinition region = new RegionDefinition(10, 40, 90, 60, 1);
        try {
            mCamera.setScanRegion(region);
        } catch (CameraEnhancerException e) {
            e.printStackTrace();
        }
      }
   }
   ```

### Initialize MRZ Recognizer

1. Import and initialize an instance of `MRZRecognizer`, bind to the created `CameraEnhancer` instance.

   ```java
   ...

   import com.dynamsoft.dlr.MRZRecognizer;
   import com.dynamsoft.dlr.LabelRecognizerException;

   public class MainActivity extends AppCompatActivity {
      
      ...

      private MRZRecognizer mRecognizer;

      @Override
      protected void onCreate(Bundle savedInstanceState) { 
         
         ...

         try {
            mRecognizer = new MRZRecognizer();
         } catch (LabelRecognizerException e) {
            e.printStackTrace();
         }

         mRecognizer.setImageSource(mCamera);
      }
   }
   ```

### Start Recognition Process

1. Override the `MainActivity.onResume` and `MainActivity.onPause` functions to start/stop video MRZ recognition. After recognition starts, the `MRZRecognizer` will automatically process the video frames from the `CameraEnhancer`, then send the recognized `MRZResult` to the callback function.

   ```java
   public class MainActivity extends AppCompatActivity {
      
      ...

      @Override
      protected void onResume() {
         mRecognizer.startScanning();
         try {
            mCamera.open();
         } catch (CameraEnhancerException e) {
            e.printStackTrace();
         }
         super.onResume();
      }


      @Override
      protected void onPause() {
         mRecognizer.stopScanning();
         try {
            mCamera.close();
         } catch (CameraEnhancerException e) {
            e.printStackTrace();
         }
         super.onPause();
      }
   }
   ```

### Obtain and Display Recognized MRZ Result

1. Create a `MRZResultListener` and register with the `MRZRecognizer` instance to get recognized MRZ result.

   ```java
   ...
   import androidx.appcompat.app.AlertDialog;
   import android.content.DialogInterface;

   import com.dynamsoft.core.ImageData;
   import com.dynamsoft.dce.DCEFeedback;
   import com.dynamsoft.dlr.MRZResult;
   import com.dynamsoft.dlr.MRZResultListener;

   public class MainActivity extends AppCompatActivity {
      
      ...

      @Override
      protected void onCreate(Bundle savedInstanceState) { 
         
         ...
         
         mMRZRecognizer.setMRZResultListener(new MRZResultListener() {
               @Override
               public void mrzResultCallback(int i, ImageData imageData, MRZResult mrzResult) {
                  if (mrzResult != null && mrzResult.isParsed) {
                     DCEFeedback.vibrate(MainActivity.this);
                     mMRZRecognizer.stopScanning();

                     runOnUiThread(() -> showResult(mrzResult));
                  }
               }
         });
      }

      private void showResult(MRZResult mrzResult) {
         StringBuilder resultBuilder = new StringBuilder();

         resultBuilder.append("Document Type : " + mrzResult.docType + "\n");
         resultBuilder.append("Issuing State : " + mrzResult.issuer + "\n");
         resultBuilder.append("Surname : " + mrzResult.surname + "\n");
         resultBuilder.append("Given Name : " + mrzResult.givenName + "\n");
         resultBuilder.append("Passport Number : " + mrzResult.docId + "\n");
         resultBuilder.append("Nationality : " + mrzResult.nationality + "\n");
         resultBuilder.append("Date of Birth(YY-MM-DD) : " + mrzResult.dateOfBirth + "\n");
         resultBuilder.append("Gender : " + mrzResult.gender + "\n");
         resultBuilder.append("Date of Expiry(YY-MM-DD) : " + mrzResult.dateOfExpiration + "\n");
         resultBuilder.append("Is Parsed : " + mrzResult.isParsed + "\n");
         resultBuilder.append("Is Verified : " + mrzResult.isVerified + "\n");
         resultBuilder.append("MRZ Text : " + mrzResult.mrzText + "\n");

         showDialog("Result", resultBuilder.toString(), new DialogInterface.OnClickListener() {
               @Override
               public void onClick(DialogInterface dialog, int which) {
                  mMRZRecognizer.startScanning();
               }
         });
      }

      private void showDialog(String title, String message, DialogInterface.OnClickListener listener) {
         new AlertDialog.Builder(this).setTitle(title)
                  .setPositiveButton("OK", listener)
                  .setMessage(message)
                  .setCancelable(false)
                  .show();
      }
   }
   ```

### Build and Run the Project

1. Select the device that you want to run your app on from the target device drop-down menu in the toolbar.
2. Click `Run app` button, then Android Studio installs your app on your connected device and starts it.
