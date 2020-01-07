# Publish with a Desktop Software - Open Broadcaster Software 

OBS(Open Broadcaster Software) is free and open source software for video recording and live streaming. You can use either your PC’s embedded camera or externally connected camera as a video source with OBS. Sound sources also can be configured. 

Let’s have a look at step by step how to use OBS for streaming:

### 1. Install the OBS 
Download Open Broadcaster Software from [obsproject.com](https://obsproject.com/) and Install it. There are distributions for Windows, Mac and Linux.

### 2. Provide Sources
Open the OBS and by default OBS starts to capture from your built-in camera if exists. You can add or remove video/audio source from Sources section. OBS is very powerful tool and it has many features. You can google about [getting professional with OBS](https://www.google.com/search?q=getting+professional+with+OBS)

![OBS (Open Broadcaster Software) interface](https://ant-media.github.io/Ant-Media-Server/doc/images/obs_screenshot.jpg)

### 3. Configure OBS
We're assuming that your Ant Media Server accepts all streams. (There is no any security option enabled.)

* Click `Settings` in the OBS Window and then Select `Stream` on the left side menu
* Choose `Custom Streaming Server` in the `Stream Type` dropdown menu.
* In the URL box, type your RTMP URL without stream id. It's like `rtmp://your_server_domain_name/LiveApp`
* In the Stream key, you can write any stream id because we assume that no security option is enabled. 

![OBS (Open Broadcaster Software) Stream Configuration](https://ant-media.github.io/Ant-Media-Server/doc/images/OBS_Configuration.png) 

### 4: Start Streaming
Close `Settings` window and just click the “Start Streaming” button in the main window of OBS.

![OBS (Open Broadcaster Software) interface](https://ant-media.github.io/Ant-Media-Server/doc/images/obs_screenshot.jpg)

Congrats. You're publishing Live Stream with OBS. 

> Quick Link: [Learn How to Play Live Streams](Playing-Live-Streams)