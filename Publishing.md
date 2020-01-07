TASLAK

## WebRTC 

### Publishing

To stream your camera with name <stream_name> to the Ant Media Server, you can open the following URL from your browser then click publish button.

<AMS_URL>:WebRTCAppEE/index.html?name=<stream_name>

### Playing

To play a stream with name <stream_name> from Ant Media Server, you can open the following URL from your browser then click play button.

<AMS_URL>:WebRTCAppEE/player.html?name=<stream_name>

If you want to start playing when stream is available, you can open following URL.

<AMS_URL>:WebRTCAppEE/play_embed.html?name=<stream_name>

### Peer-to-peer

If you want to use Ant Media Server for a signalling server you open following URL.

<AMS_URL>:WebRTCAppEE/peer.html?name=<stream_name>

### Conference

If you want to use Ant Media Server for a multiuser conference, you open following URL.

<AMS_URL>:WebRTCAppEE/conference.html

## WebRTC Screen Sharing

Seamless switch between WebRTC Screen Sharing & Camera is available on both Community and Enterprise Edition. We mean by seamless is that you can switch between Screen & Camera in the same session.

### Try WebRTC Screen Sharing

Firstly, just let me tell how to try it. After that I will give some information about JavaScript API and chrome extension.

 Install Ant Media Server to your server in the cloud with Getting Started. In order to download community edition, visit main page or if you want to try Enterprise edition, keep in touch.

 Go to WebRTC publishing web page which is https://FQDN:5443/WebRTCApp (Community E. ) or https://FQDN:5443/WebRTCAppEE (Enterprise E.). By the way,  you need to assign a domain to your server and install SSL. Otherwise, chrome does not let you access the camera or screen.

Install Chrome Extension by clicking on the link or directly from Chrome WebStore. Luckily, source code of the extension is also available on github.

 Return back to WebRTC Publish page and click Screen Share Checkbox

Screen is going to be shared on the video element

Click “Start Publishing” and also check/uncheck “Screen Share” box while publishing in the same session. MP4 file or HLS stream will record the screen or your camera according to your preference. You can watch the stream live on web panel via HLS.

### Implementing WebRTC Screen Sharing

We just add some simple functions to js/webrtc_adaptor.js file to seamless switch between screen sharing and camera. You can take a look at the source code of WebRTCApp/index.html  to see the full implementation. Meanwhile, let me give some highlights about JavaScript API.

* Firstly, There is a new callback with “screen_share_extension_available”. If callback function is called with this parameter, it means that Ant Media Server Screen Share extension is ready to use in the Chrome.

    var webRTCAdaptor = new WebRTCAdaptor({
    websocket_url : websocketURL,
    mediaConstraints : mediaConstraints,
    peerconnection_config : pc_config,
    sdp_constraints : sdpConstraints,
    localVideoId : "localVideo",
    debug:true,
    callback : function(info, description) {
         if (info == "initialized") {
        console.log("initialized");

         } else if (info == "publish_started") {
        //stream is being published
        console.log("publish started");
         } else if (info == "publish_finished") {
        //stream is being finished
        console.log("publish finished");
         }
         else if (info == "screen_share_extension_available") {
        console.log("screen share extension available");
          }
          else if (info == "closed") {
          //console.log("Connection closed");
          if (typeof description != "undefined") {
            console.log("Connecton closed: " + JSON.stringify(description));
          }
          }
},
    callbackError : function(error, message) {
        //some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError

            console.log("error callback: " +  JSON.stringify(error));
    }
});

* Secondly, if extension is available in the Chrome, you only need to call “webRTCAdaptor.switchDesktopCapture(streamId);” function to switch the Screen Sharing

webRTCAdaptor.switchDesktopCapture(streamId);

* Lastly, to switch back to camera,  just again call to “webRTCAdaptor.switchVideoCapture(streamId);” function.

webRTCAdaptor.switchVideoCapture(streamId);

* Finally, that’s all. I hope this blog post will help some guys. You can use this feature on your project.

If you have any question, please keep in touch.

