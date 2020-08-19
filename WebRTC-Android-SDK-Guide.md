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

* ### Publish Stream from your Android

  * You need to set `webRTCMode` parameter in `IWebRTCClient.MODE-PUBLISH`

  * Set the stream id to anything else then 'stream1' and Tap 'Connect' button on the main screen. Then it will ask you to access the Camera and Mic. After you allow the Camera and Mic access, stream will be published on Ant Media Server.

    <img src="https://antmedia.io/wp-content/uploads/2020/08/android-webrtc-sdk-play-screen.jpg" width=360 />

  * Then it will start Publishing to your Ant Media Server. You can go to the web panel of Ant Media Server(http://server_ip:5080) and watch the stream there. You can also quickly play the stream via https://your_domain:5443/WebRTCAppEE/player.html

* ### Play Stream from your Android

  * Firtsly, you need to set `webRTCMode` parameter in `IWebRTCClient.MODE_PLAY`.

  * Playing stream on your Android is almost the same as Publishing. Before playing, make sure that there is a stream is already publishing to the server with same stream id in your `streamId` parameter (You can quickly publish to the Ant Media Server via https://your_domain:5443/WebRTCAppEE). For our sample, stream id is still "stream1" in the image below. Then you just need to tap 'Start Playing' button.

    <img src="https://antmedia.io/wp-content/uploads/2020/08/android-webrtc-sdk-publish-screen.jpg" width=360 />




