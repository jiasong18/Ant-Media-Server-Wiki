Seamless switch between WebRTC Screen Sharing & Camera is available on both Community and Enterprise Edition. We mean by seamless is that you can switch between Screen & Camera in the same session.

<!-- Picture -->

### Try WebRTC Screen Sharing without plugin

Firstly, you should have `getDisplayMedia` supported browser. You can see `getDisplayMedia` supported browser list in [here](https://caniuse.com/#search=getDisplayMedia).

Install Ant Media Server to your server in the cloud with <a href="https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started#installation">Getting Started</a>. In order to download community edition, visit <a href="https://antmedia.io">main page</a> or if you want to try Enterprise edition,<a href="https://antmedia.io/#contact"> keep in touch.</a>

Go to WebRTC publishing web page which is `https://domainAddress:5443/WebRTCApp` (Community Edition ) or `https://domainAddress:5443/WebRTCAppEE` (Enterprise Edition). By the way,  you need to assign a domain to your server and <a href="https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup">install SSL. </a>Otherwise, chrome does not let you access the camera or screen.

### Implementing WebRTC Screen Sharing

You can see simple functions to [js/webrtc_adaptor.js](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/js/webrtc_adaptor.js) file to seamless switch between screen sharing without plugin and camera. You can take a look at the source code of <a href="https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/index.html">WebRTCApp/index.html</a>  to see the full implementation.



Firstly, There is a new callback with "screen_share_extension_available". If callback function is called with this parameter, it means that Ant Media Server Screen Share extension is ready to use in the Chrome.

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

** Switch to Screen Share **

If your browser is support in `getDisplayMedia`, you only need to call `webRTCAdaptor.switchDesktopCapture(streamId);` function to switch the Screen Sharing
> `webRTCAdaptor.switchDesktopCapture(streamId);`


** Switch back to Camera **

Switch back to camera, just again call to `webRTCAdaptor.switchVideoCapture(streamId);` function.
> `webRTCAdaptor.switchVideoCapture(streamId);`

**Switch to Screen Share with Camera**

You should change some definition in `mediaConstraint` as below 

```javascript

	var mediaConstraints = {
		video : "screen+camera",
		audio : true
	}; 

```

After the changing in `mediaConstraints` you only need to call `webRTCAdaptor.switchDesktopCapture(streamId);` function to switch the Screen Sharing
> `webRTCAdaptor.switchDesktopCapture(streamId);`