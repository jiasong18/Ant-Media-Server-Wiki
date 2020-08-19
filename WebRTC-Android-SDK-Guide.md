Ant Media's WebRTC Android SDK lets you build your own Android application that can publish and play WebRTC broadcasts with just a few lines of code.   
In this doc, we're going to cover the following topics. 
* Run the Sample WebRTC Android app
  * Publish Stream from your Android
  * Play Stream on your Android
  * P2P Communication with your Android
* Develop a WebRTC Android app
  * How to Publish
  * How to Play
  * How to use DataChannel

# Run the Sample WebRTC Android app

* ### Download the WebRTC Android SDK
  WebRTC iOS and Android SDK's are free to download. You can access them through [this link on antmedia.io](https://antmedia.io/free-webrtc-android-ios-sdk/). If you're an enterprise user, it will be also available for you to download in your subscription page. Anyway, after you download the SDK, you can just unzip the file and open the project with Android. 

* ### Open and Run the Project in Android

Just Click Open an Existing Android Studio Project. A window should open as shown below for the project path.

![Android Project Path](images/android-open-project-path.png)

Select your project in a path and Click to the OK button.

![Android project path apply](images/android-project-path-apply.png)

> PS: You need to set `SERVER_ADDRESS` parameter in MainActivity.java

## Publish Stream from your Android

  * You need to set `webRTCMode` parameter in `IWebRTCClient.MODE-PUBLISH`

  * Set the stream id to anything else then `stream1` and Tap `Start Publishing` button on the main screen. After the click `Start Publishing`, stream will be published on Ant Media Server.

    <img src="https://antmedia.io/wp-content/uploads/2020/08/android-webrtc-sdk-play-screen.jpg" width=360 />

  * Then it will start Publishing to your Ant Media Server. You can go to the web panel of Ant Media Server(http://server_ip:5080) and watch the stream there. You can also quickly play the stream via `https://your_domain:5443/WebRTCAppEE/player.html`

## Play Stream from your Android

  * Firtsly, you need to set `webRTCMode` parameter in `IWebRTCClient.MODE_PLAY`.

  * Playing stream on your Android is almost the same as Publishing. Before playing, make sure that there is a stream is already publishing to the server with same stream id in your `streamId` parameter (You can quickly publish to the Ant Media Server via `https://your_domain:5443/WebRTCAppEE`). For our sample, stream id is still "stream1" in the image below. Then you just need to tap `Start Playing` button.

    <img src="https://antmedia.io/wp-content/uploads/2020/08/android-webrtc-sdk-publish-screen.jpg" width=360 />

## P2P Communication with your Android

  WebRTC Android SDK also supports P2P communication. As you guess, just set `webRTCMode` parameter to `IWebRTCClient.MODE_JOIN`.

  <img src="https://antmedia.io/wp-content/uploads/2020/08/android-webrtc-sdk-peer-screen.jpg" width=360 />

  When there is another peer is connected to the same stream id via Android, iOS or Web, then P2P communication will be established and you can talk each other. You can quick connect to the same stream id via `https://your_domain:5443/WebRTCAppEE/peer.html`

# Develop a WebRTC Android app

We highly recommend using the sample project to get started your application. Nevertheless, it's good to know the dependencies and how it works. So that we're going to tell how to create a WebRTC Android app from Scratch. Let's get started. 

## Creating Android Project
 
### Open Android Studio and Create a New Android Project
Just Click **`File > New > New Project`** . A window should open as shown below for the project details. Fill the form according to your Organization and Project Name
<img alt="Create Android Studio Project For WebRTC Native Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.05.59.png" width="800px" />

Click Next button and Choose **`Phone and Tablet`** as below. 
<img alt="WebRTC Native Android App" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.06.08.png" />

Lastly Choose **`Empty Activity`** in the next window
<img alt="Choose Empty Activity" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.19.11.png" width="800px" />

Let the default settings for activity name and layout. Click Next and finish creating the project.

### Import WebRTC SDK as Module to Android Project 

After creating the project. Let's import the WebRTC Android SDK to the project. For doing that click
**`File > New > Import Module`** . Choose the directory of the WebRTC Android SDK and click the Finish button.
<img alt="Import Native WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.11.49.png" width="800px" />

If module is not included in the project, add the module name into `settings.gradle` file as shown in the image below.
<img alt="Import module in setting.gradle" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.20.22-3.png" width="800px" />

### Add dependency to Android Project App Module

Right-click `app`, choose `Open Module Settings`and click the `Dependencies` tab. Then a window should appear as below. Click the `+` button at the bottom and choose `Module Dependency``
<img alt="Add Module Dependency WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.34.56.png" width="800px" />

Choose WebRTC Native Android SDK and click OK button

<img alt="Native WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.21.06.png" width="800px" />

#### **CRITICAL thing about that** You need import Module as an **API** as shown in the image below. You can change it from **Implementation** to **API** in the drop-down list

<img alt="Choose API in drop down list" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.39.27.png" width="800px" />

## Prepare the App for Streaming

  * Create a MainActivity.java Class and add a Button to your activity main layout. This is just simple Android App development, we don't give details here. You can get lots of tutorials about that on the Internet.

  * Add permissions in Manifest file.

Open the AndroidManifest.xml and add below permissions between `application` and `manifest`tag
```xml
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
    <uses-feature
        android:glEsVersion="0x00020000"
        android:required="true" />

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

<img alt="Set permissions for the App" src="https://antmedia.io/wp-content/uploads/2020/08/android-manifest-permissions.png" width="800px" />

### Implement MainActivity onCreate function

Open the MainActivity.java and implement it as below. You should change `SERVER_URL` according to your Ant Media Server address. Secondly, the third parameter in the last line of the code below is `IWebRTCClient.MODE_PUBLISH` that publishes the stream to the server. You can use `IWebRTCClient.MODE_PLAY` for playing stream and `IWebRTCClient.MODE_JOIN` for P2P communication. If token control is enabled, you should define `tokenId` parameter.

## How to Publish

  * It's now to write some code. Initialize `webRTCClient` in `MainActivity.java`



