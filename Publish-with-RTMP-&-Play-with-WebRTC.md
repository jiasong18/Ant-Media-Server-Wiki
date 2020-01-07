***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Developers can make their app users broadcast live video not only from their browser with WebRTC, but also from OBS, thanks to WebRTC Adapter. Let’s start with introducing OBS which stands for Open Broadcaster Software. OBS is a free open source software for video recording and live streaming. You can use either your PC’s embedded camera or externally connected one as a video source with OBS. Sound sources also can be configured with it. Ant Media Server Enterprise is fully compatible with OBS.

Let’s have a look at step by step how to use OBS for streaming:
### Step 1: Getting the OBS:

Download via its official [web page](https://obsproject.com/). It has Windows, Mac, and Linux releases.

### Step 2 : Provide Sources:
![OBS Image](https://i0.wp.com/antmedia.io/wp-content/uploads/2017/12/Screenshotfrom2017-12-2116-16-04-1024x576.jpg)

RTMP stream from OBS for WebRTC replay

By default, OBS starts to capture from your embedded camera if exists after initialized. You can add or remove video/audio source from Sources section, such as an image can be broadcasted as a video source or external microphone can be added as an audio source.

### Step 3: Configure the Ant Media Server

Contact with us from [contact form](https://antmedia.io/#contacts) in order to try Ant Media Server Enterprise Edition. Extract the Ant Media Server Enterprise zip file and start the server with start.sh command in the terminal. For detailed information please follow steps described in the [Getting Started](https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started).
```
cd /path/to/ant-media-server

./start.sh
```

After that, you need to create a live stream on Ant Media Server Enterprise Management console. You can reach it at `http://<server-address>:5080` address, then click `WebRTCAppEE` from Applications section and `New Live Stream`. The server creates a live stream with an unique ID with the format of `rtmp://<server-address>/WebRTCAppEE/325859929809451108600212`. You can copy this url with clicking clipboard icon.
 

Ant Media Server Dashboard
![Ant Media Server Dashboard](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/08/webrtcAppEE.png)

### Step 4: Configure the OBS

You need to write live stream parameters to OBS in order to start broadcasting. Click `Settings` then select `Stream` tab. Split the `rtmp://<server-address>/WebRTCAppEE/325859929809451108600212` url and and write url and stream key parameters as described in the below image. Make sure that Stream ID should be written to Stream Key field, not to the url.

OBS (Open Broadcaster Software) Stream Configuration
![OBS and Ant Media Server](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/08/obs_set_stream_url.png)

**If you want to decrease latency as well, you can `tune` encoder for `zerolatency` in encoder settings.**
 

### Step 5 : Start Stream and Watch 🙂

Close settings window and just click the `Start Streaming` button in the main window of OBS. You can watch WebRTC stream from either Ant Media Management console or other platforms such as VLC player with same RTMP URL.

To Play with WebRTC, go to `http://<server-address>:5080/WebRTCAppEE/player.html` and enter the stream key parameter and click "Start Playing" button.


![WebRTC Play - Ant Media Server](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/08/player.png)


You can also play on Ant Media Management console which plays via HLS at about 8-10 seconds latency. A dialog will be displayed where you can watch the live stream.

![Ant Media Server Management Console ](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/08/console_play.png)

In order to reduce latency in RTMP streaming, please [follow the instructions here](https://github.com/ant-media/Ant-Media-Server/wiki/How-to-Reduce-Latency-in-RTMP-to-HLS%3F)

We hope this tutorial will be helpful for you 🙂 please feel free in case you have any question, just send an email (contact at antmedia.io) or contact with us from contact page.