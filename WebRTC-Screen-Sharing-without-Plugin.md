Seamless switch between WebRTC Screen Sharing & Camera is available on both Community and Enterprise Edition. It means that you can switch between Screen & Camera in the same session.

![WebRTC Screen Share without Plugin](images/webrtc-screen-share-without-plugin.png)

## Try WebRTC Screen Sharing without plugin

Firstly, you should have `getDisplayMedia` supported browser. You can see `getDisplayMedia` supported browser list in [here](https://caniuse.com/#search=getDisplayMedia).

Visit the WebRTC publishing web page which is `https://domainAddress:5443/WebRTCApp` (Community Edition ) or `https://domainAddress:5443/WebRTCAppEE` (Enterprise Edition). By the way, you need to assign a domain to your server and install SSL. Otherwise, chrome does not let you access the camera or screen.

> Quick Link: [Learn How to Install SSL to your Ant Media Server](https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup)

## Using WebRTC Screen Sharing 

You can see simple functions to [js/webrtc_adaptor.js](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/js/webrtc_adaptor.js) file to seamless switch between screen sharing and camera. 

You can take a look at the source code of <a href="https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/index.html">WebRTCApp/index.html</a>  to see the full implementation.

Firstly, there is a new callback with `browser_screen_share_supported`. If callback method is called with this parameter, it means that your browser Screen Share is ready to use.

```javascript

	var webRTCAdaptor = new WebRTCAdaptor({
		websocket_url : websocketURL,
		mediaConstraints : mediaConstraints,
		peerconnection_config : pc_config,
		sdp_constraints : sdpConstraints,
		localVideoId : "localVideo",
		debug:true,
		callback : function(info, obj) {
			if (info == "initialized") {
				console.log("initialized");
				start_publish_button.disabled = false;
				stop_publish_button.disabled = true;
			} else if (info == "publish_started") {
				//stream is being published
				console.log("publish started");
				start_publish_button.disabled = true;
				stop_publish_button.disabled = false;
				startAnimation();
			} else if (info == "publish_finished") {
				//stream is being finished
				console.log("publish finished");
				start_publish_button.disabled = false;
				stop_publish_button.disabled = true;
			}
			else if (info == "browser_screen_share_supported") {
				screen_share_checkbox.disabled = false;
				console.log("browser screen share supported");
				browser_screen_share_doesnt_support.style.display = "none";
			}
			else if (info == "screen_share_stopped") {
				console.log("screen share stopped");
			}
			else if (info == "closed") {
				//console.log("Connection closed");
				if (typeof obj != "undefined") {
					console.log("Connecton closed: " + JSON.stringify(obj));
				}
			}
			else if (info == "pong") {
				//ping/pong message are sent to and received from server to make the connection alive all the time
				//It's especially useful when load balancer or firewalls close the websocket connection due to inactivity
			}
			else if (info == "refreshConnection") {
				startPublishing();
			}
			else if (info == "ice_connection_state_changed") {
				console.log("iceConnectionState Changed: ",JSON.stringify(obj));
			}
			else if (info == "updated_stats") {
				//obj is the PeerStats which has fields
				 //averageOutgoingBitrate - kbits/sec
				//currentOutgoingBitrate - kbits/sec
				console.log("Average outgoing bitrate " + obj.averageOutgoingBitrate + " kbits/sec"
						+ " Current outgoing bitrate: " + obj.currentOutgoingBitrate + " kbits/sec");
				 
			}
		},
		callbackError : function(error, message) {
			//some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError
            
			console.log("error callback: " +  JSON.stringify(error));
			var errorMessage = JSON.stringify(error);
			if (typeof message != "undefined") {
				errorMessage = message;
			}
			var errorMessage = JSON.stringify(error);
			if (error.indexOf("NotFoundError") != -1) {
				errorMessage = "Camera or Mic are not found or not allowed in your device";
			}
			else if (error.indexOf("NotReadableError") != -1 || error.indexOf("TrackStartError") != -1) {
				errorMessage = "Camera or Mic is being used by some other process that does not let read the devices";
			}
			else if(error.indexOf("OverconstrainedError") != -1 || error.indexOf("ConstraintNotSatisfiedError") != -1) {
				errorMessage = "There is no device found that fits your video and audio constraints. You may change video and audio constraints"
			}
			else if (error.indexOf("NotAllowedError") != -1 || error.indexOf("PermissionDeniedError") != -1) {
				errorMessage = "You are not allowed to access camera and mic.";
			}
			else if (error.indexOf("TypeError") != -1) {
				errorMessage = "Video/Audio is required";
			}
			else if (error.indexOf("ScreenSharePermissionDenied") != -1) {
				errorMessage = "You are not allowed to access screen share";
				screen_share_checkbox.checked = false;
			}
			else if (error.indexOf("WebSocketNotConnected") != -1) {
				errorMessage = "WebSocket Connection is disconnected.";
			}
			alert(errorMessage);
		}
	});

```

### Switch to Desktop Screen Share

If your browser supports  `getDisplayMedia`, you only need to call `webRTCAdaptor.switchDesktopCapture(streamId);` function to switch the Screen Sharing
> `webRTCAdaptor.switchDesktopCapture(streamId);`

Please take a look at the Sample page code at [index.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/index.html).

### Switch to Screen Share with Camera

You only need to call `webRTCAdaptor.switchDesktopCaptureWithCamera(streamId);` function to switch the Screen Sharing.
> `webRTCAdaptor.switchDesktopCaptureWithCamera(streamId);`

### Switch back to Camera

Switch back to camera, just again call to `webRTCAdaptor.switchVideoCapture(streamId);` function.
> `webRTCAdaptor.switchVideoCapture(streamId);`

Please take a look at the Sample pages code at [camera_plus_screen.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/camera_plus_screen.html) and [index.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/index.html).
