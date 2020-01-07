Adaptive bitrate streaming(aka. dynamic adaptive streaming, multi-bitrate streaming) allows you to deliver the optimum video quality according to the network bandwidth between you and media server. This enables users can play the videos smoothly regardless of their internet connection speed or device.

## Why to Use Adaptive Bitrate?

More people are getting connected to the internet and more videos are being watched on each day. In other words, each of us watching more videos on each day.

But there may be an issue about watching video on internet because internet connection speeds usually does not let watching videos in high quality so that player needs buffering and this make you to wait while watching video.

![](https://raw.githubusercontent.com/mekya/antmedia-doc/master/images/buffering.jpg)

In order to give better user experience,  service providers create lower resolutions of the videos to make people watch videos seamlessly even if their network condition is not good enough to watch HD videos. So that you do not wait player to buffer in adaptive streaming.

![](https://raw.githubusercontent.com/mekya/antmedia-doc/master/images/AP658325161480_131.jpg)

## Adaptive Bitrate on the Fly
Lowering the resolutions of videos for recorded streams is not a big deal. However, doing the same job for live streams on the fly is not as easy as recorded streams. Nevertheless, Ant Media Server supports adaptive bitrate streaming in Enterprise Edition and live streams can be played with WebRTC and HLS(Http Live Streaming).

![HLS Adaptive Streaming Schema](https://raw.githubusercontent.com/mekya/antmedia-doc/master/images/HLSsegmentedvideodelivery.png)

## How WebRTC & HLS Adaptive Streaming Works
Ant Media Server supports adaptive streaming in both WebRTC and HLS formats. On the other hand, there is a _slight difference between WebRTC and HLS adaptive streaming_. In WebRTC, Ant Media Server measures the player's bandwidth and selects the optimum quality according to the player's bandwidth. On the contrary, in HLS, player measures the its bandwidth and fetches the optimum quality from the server.    

## How to enable Adaptive Bitrate

### 1. Web Panel
* `App > Settings > Adaptive Bitrate`

![](https://raw.githubusercontent.com/mekya/antmedia-doc/master/images/abs.png)

> **Note :** Adaptive streaming detects the bandwidth and CPU capacity of the device and adjusts the streaming rate and video quality appropriately. Therefore it will be high cpu load. We recommend to use GPU.

> Quick Link: [Learn How to Enable GPU for Ant Media Server](GPU)  

The configuration above will create videos at 1080p, 720p and 360p resolutions if the incoming stream’S resolution is higher than 1080p. If the incoming stream's resolution is 480p, then 480p and 360p versions of the stream will be created on the fly.

![](https://raw.githubusercontent.com/mekya/antmedia-doc/master/images/iosmediacaptureresolutions.png)

### 2. Configuration file

* Open your configuration file for your favorite editor.
`vim {INSTALL_DIR}/webapps/{APP_NAME}/WEB-INF/red5-web.properties`

* Add following lines
`settings.encoderSettingsString=360,800000,64000,240,500000,32000` 

Format is `Resolution Height`,`Video Bitrate Per Second`, `Audio Bitrate Per Second`.
So that the string above add two adaptive bitrates. 
  1. 360p, 800Kbps Video Bitrate, 64Kbps Audio Bitrate 
  2. 240p, 500Kbps Video Bitrate, 32Kbps Audio Bitrate
 
* Save and close file. After manual adaptive bitrate settings, you need to restart the Ant Media Server
````
sudo service antmedia restart
````