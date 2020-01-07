***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Ant Media Server 1.2.0+ Enterprise Edition supports adaptive low latency WebRTC streaming. 

In addition, Ant Media Server can
* Record WebRTC streams as MP4 and MKV
* Convert WebRTC streams to adaptive live HLS
* Create previews in PNG format from WebRTC streams

## Download

Firstly, you need to have Ant Media Server Enterprise Edition. If you are a personal user and just want to try,
contact with us at [antmedia.io](https://antmedia.io). We will reply back by providing Ant Media Server Enterprise Edition to try. 

If you are a professional user and need support, you can buy support at [antmedia.io](https://antmedia.io) as well

## Quick Start

*Ant Media Server 1.2.0+ runs on Linux and Mac not on Windows.* 

Let's start, we assume that you have got Enterprise Edition somehow and downloaded to your local computer 

1. Follow the instructions on [Getting Started] for installation (https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started) 

 
2. Open the browser(Chrome or Firefox) and go to the `http://localhost:5080/WebRTCAppEE`. 
    Let browser access your camera and mic unless it cannot send WebRTC Stream to the server
    
    ![Open WebRTCAppEE](images
    
    _WebRTCAppEE stands for WebRTCApp Enterprise Edition_
   
3. Write stream name or leave it as default and Press `Start Publishing` button. After you press the button, 
    "Publishing" blinking text should appear

    ![Press Start Publishing button](images

4. Go to the `http://localhost:5080/WebRTCAppEE/player.html`

    ![Go to the player.html](images

5. Press `Start Play` button. After you press the button, webrtc stream should be started

    ![Press Start Playing Button](images
    
6. Open `http://localhost:5080/WebRTCAppEE/player.html` in other tabs and Press `Start Playing` button again 
   to check how it plays and what the latency is. 
   
## Running on Remote Instances - Enable SSL For Ant Media Server Enterprise
If you are running the Ant Media Server Enterprise Edition in remote computer/instances, you may need SSL. If so, please follow the Enable SSL doc or [this blog post](https://antmedia.io/enable-ssl-on-ant-media-server/).

## Feedbacks

Please let us know your feedbacks about the latency and streaming or any other issue you have faced 
so that we can improve and let you try and use.

You can use contact form at [antmedia.io](https://antmedia.io) or `contact at antmedia dot io` e-mail to send your feedbacks

Thank you

[Ant Media](https://antmedia.io)