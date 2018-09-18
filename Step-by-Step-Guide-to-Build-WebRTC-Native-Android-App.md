WebRTC Native Android SDK lets developers build their own native WebRTC applications that with just some clicks. WebRTC Android SDK has built-in functions such as publishing to Ant Media Server for one-to-many streaming, playing a stream in Ant Media Server and lastly P2P communication by using Ant Media Server as a signalling server. Anyway, no need to say too much things let's make our hands dirty. 

### Download the WebRTC Native Android SDK
We provide WebRTC Native Android SDK to Ant Media Server Enterprise users for free. Please keep in touch for getting WebRTC Native Android SDK. As a result download the SDK and open it to a directory that we will import it to Android App Project
    
## Creating Android Project
### Open Android Studio and Create a New Android Project
Just Click **`File > New > New Project`** . A window should open as shown below for the project details. Fill the form according to your Organization and Project Name
<img alt="Create Android Studio Project For WebRTC Native Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.05.59.png" width="800px" />

Click Next button and Choose **`Phone and Tablet`** as below. 
<img alt="WebRTC Native Android App" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.06.08.png" width="800px" />

Lastly Choose **`Empty Activity`** in the next window
<img alt="Choose Empty Activity" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.19.11.png" width="800px" />

Let the default settings for activity name and layout. Click Next and finish creating the project.

## Import WebRTC SDK to Android Project 
After creating the project. Let's import the WebRTC Android SDK to the project. For doing that click
**`File > New > Import Module`** . Choose directory of the WebRTC Android SDK and click Finish button.
<img alt="Import Native WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.11.49.png" width="800px" />

If module is not included in the project, add the module name into `settings.gradle` file as shown in the image below.
<img alt="Import module in setting.gradle" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.20.22-3.png" width="800px" />

## Add dependency to Android Project App Module
Right click `app`, choose `Open Module Settings`and click the `Dependencies` tab. Then a window should appear as below. Click the `+` button at the bottom and choose `Module Dependency``
<img alt="Add Module Dependeny WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.34.56.png" width="800px" />

Choose WebRTC Native Android SDK and click OK button

<img alt="Native WebRTC Android SDK" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-08.21.06.png" width="800px" />

#### **CRITICAL thing about that** You need import Module as an **API** as shown in the image below. You can change it from **Implementation** to **API** in the drop down list

<img alt="Choose API in drop down list" src="https://antmedia.io/wp-content/uploads/2018/07/Screen-Shot-2018-07-27-at-09.39.27.png" width="800px" />

So let's do other simple stuff. 

## Prepare the App for Stream Publishing 
### Set permissions for the App. 
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

### Implement MainActivity `onCreate` function
Open the MainActivity and implement it as below. You should change `SERVER_URL` according to your Ant Media Server address. Secondly, the third parameters in the last line of the code below is `IWebRTCClient.MODE_PUBLISH` that publishes stream to the server. You can use `IWebRTCClient.MODE_PLAY` for playing stream and `IWebRTCClient.MODE_JOIN` for P2P communication. If token control is enabled, you should define `tokenId` parameter.

```java
public class MainActivity extends AbstractWebRTCActivity {

    public static final String SERVER_URL = "ws://192.168.1.21:5080/WebRTCAppEE/websocket";
    private CallFragment callFragment;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //below exception handler show the exception in a popup window
        //it is better to use in development, do not use in production
        Thread.setDefaultUncaughtExceptionHandler(new UnhandledExceptionHandler(this));

        // Set window styles for fullscreen-window size. Needs to be done before
        // adding content.
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN | WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON
                | WindowManager.LayoutParams.FLAG_DISMISS_KEYGUARD | WindowManager.LayoutParams.FLAG_SHOW_WHEN_LOCKED
                | WindowManager.LayoutParams.FLAG_TURN_SCREEN_ON);
        getWindow().getDecorView().setSystemUiVisibility(getSystemUiVisibility());

        setContentView(R.layout.activity_main);

        webRTCClient = new WebRTCClient( this,this);

        String streamId = "stream" + (int)(Math.random() * 999);
        String tokenId = "tokenID";
        callFragment = new CallFragment();
        callFragment.setCallEvents(webRTCClient);
        callFragment.setStreamId(streamId);
        FragmentTransaction ft = getFragmentManager().beginTransaction();
        ft.add(R.id.call_fragment_container, callFragment);
        ft.commit();

        SurfaceViewRenderer cameraViewRenderer = findViewById(R.id.camera_view_renderer);

        webRTCClient.setFullScreenRenderer(cameraViewRenderer);

        checkPermissions();
     
        //streamId is randomly assigned and it will be shown to the screen to watch it on Ant Media Server
        webRTCClient.startStream(SERVER_URL, streamId, IWebRTCClient.MODE_PUBLISH, tokenId);

    }
}
```

### Edit the `activity_main.xml` as below
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <org.webrtc.SurfaceViewRenderer
        android:id="@+id/camera_view_renderer"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center" />

    <FrameLayout
        android:id="@+id/call_fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent" /> 
</FrameLayout>
```
### Build and Start the App
App directly publishes stream to the server before that we need to let the app has the permissions for that. Make sure that you let the app has permissions as shown below.

<img alt="Permissions" src="https://antmedia.io/wp-content/uploads/2018/07/Screenshot_2018-07-27-08-56-45.png"   height="800px" /> 

Then restart the app and it should open the camera and start streaming. You should the see stream id in the screen as below. You can go to the `http://SERVER_URL:5080/WebRTCAppEE/player.html`, write stream id to the text box and click Play button.

<img alt="Publish with WebRTC" src="https://antmedia.io/wp-content/uploads/2018/07/publishing_webrtc.png" height="800px" /> 


