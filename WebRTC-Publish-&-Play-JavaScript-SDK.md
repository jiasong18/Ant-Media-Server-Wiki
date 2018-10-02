Ant Media Server provides WebSocket interface in publishing and playing WebRTC streams. In this document, 
we will show both how to publish and play WebRTC streams by using JavaScript SDK. 

## How to Publish WebRTC stream with JavaScript SDK
Let's see how to make it step by step

1. Load the below scripts in head element of the html file
```html
<head>
...
<script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
<script src="js/webrtc_adaptor.js" ></script>
...
</head>
```

2. Create local video element somewhere in the body tag
```html
<video id="localVideo" autoplay muted width="480"></video>
```


3. Initialize the `WebRTCAdaptor` object in script tag

```javascript
	var pc_config = null;

	var sdpConstraints = {
		OfferToReceiveAudio : false,
		OfferToReceiveVideo : false

	};
	var mediaConstraints = {
		video : true,
		audio : true
	};

	var webRTCAdaptor = new WebRTCAdaptor({
		websocket_url : "ws://" + location.hostname + ":8081/WebRTCAppEE",
		mediaConstraints : mediaConstraints,
		peerconnection_config : pc_config,
		sdp_constraints : sdpConstraints,
		localVideoId : "localVideo",
		callback : function(info) {
			if (info == "initialized") {
				console.log("initialized");
				
			} else if (info == "publish_started") {
				//stream is being published 
				console.log("publish started");
				
				
			} else if (info == "publish_finished") {
				//stream is finished
				console.log("publish finished");
				
			}
		},
		callbackError : function(error) {
			//some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError

			console.log("error callback: " + error);
			alert(error);
		}
	});
```

4. Call `publish(streamName)` to Start Publishing


In order to publish WebRTC stream to Ant Media Server, WebRTCAdaptor's `publish(streamName)` function should be called. 
You can choose the call this function in *success callback function* when the info parameter is having value "initialized" 

```javascript
 if (info == "initialized")  
 {  
  // it is called with this parameter when it connects to                            
  // Ant Media Server and everything is ok 
  console.log("initialized");
  webRTCAdaptor.publish("stream1");
 }
```
5. Call `stop()` to Stop Publishing

You may want to stop publishing anytime by calling stop function of WebRTCAdaptor

```javascript
webRTCAdaptor.stop()
```

#### Sample
Please take a look at the WebRTCAppEE/index.html file in order to see How JavaScript SDK can be used for publishing a stream

## How to Play WebRTC stream with JavaScript SDK

1. Load the below scripts in head element of the html file
```html
<head>
...
<script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
<script src="js/webrtc_adaptor.js" ></script>
...
</head>
```

2. Create remote video element somewhere in the body tag
```html
<video id="remoteVideo" autoplay controls></video>
```


3. Initialize the `WebRTCAdaptor` object like below in script tag
```javascript
  var pc_config = null;

	var sdpConstraints = {
		OfferToReceiveAudio : true,
		OfferToReceiveVideo : true

	};
	var mediaConstraints = {
		video : true,
		audio : true
	};
	
	var webRTCAdaptor = new WebRTCAdaptor({
		websocket_url : "ws://" + location.hostname + ":8081/WebRTCAppEE",
		mediaConstraints : mediaConstraints,
		peerconnection_config : pc_config,
		sdp_constraints : sdpConstraints,
		remoteVideoId : "remoteVideo",
		isPlayMode: true,
		callback : function(info) {
			if (info == "initialized") {
				console.log("initialized");
			
			} else if (info == "play_started") {
				//play_started
				console.log("play started");
			
			} else if (info == "play_finished") {
				// play finishedthe stream
				console.log("play finished");
				
			}
		},
		callbackError : function(error) {
			//some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError

			console.log("error callback: " + error);
			alert(error);
		}
	});
```  
4. Call `play(streamName)` to Start Playing


In order to play WebRTC stream to Ant Media Server, WebRTCAdaptor's `play(streamName)` function should be called. 

You can choose the call this function in *success callback function* when the info parameter is having value "initialized" 

```javascript
 if (info == "initialized")  
 {  
  // it is called with this parameter when it connects to                            
  // Ant Media Server and everything is ok 
  console.log("initialized");
  webRTCAdaptor.play("stream1");
 }
```

5. Call `stop()` to Stop Playing

You may want to stop play anytime by calling stop function of WebRTCAdaptor

```javascript
webRTCAdaptor.stop()
```
## JavaScript Error Callbacks 

  * **`WebSocketNotSupported`**: WebSocket connection is not supported for environment or connection is not in the correct state.

  * **`AbortError`**: Although the user and operating system both granted access to the hardware device, and no hardware issues occurred that would cause a NotReadableError, some problem occurred which prevented the device from being used.

 * **`NotAllowedError`**: The user has specified that the current browsing instance is not permitted access to the device, or the user has denied access for the current session, or the user has denied all access to user media devices globally.

 * **`NotFoundError`**: No media tracks of the type specified were found that satisfy the given constraints.

 * **`OverconstrainedError`**: The specified constraints resulted in no candidate devices which met the criteria requested. The error is an object of type OverconstrainedError and has a constraint property whose string value is the name of a constraint which was impossible to meet, and a message property containing a human-readable string explaining the problem.

 * **`SecurityError`**: User media support is disabled on the Document on which getUserMedia() was called. The mechanism by which user media support is enabled and disabled is left up to the individual user agent.

 * **`AudioAlreadyActive`**: If there is audio it calls callbackError with "AudioAlreadyActive.

 * **`Camera or Mic is being used by some other process that does not let read the devices`**: It is sent when media devices are used by another application.

 * **`VideoAlreadyActive`**: If there is video it calls callbackError with "VideoAlreadyActive.

 * **`NotSupportedError`**: It is sent when SSL is needed.

 * **`noStreamNameSpecified`**: It is sent when stream id is not specified in the message.

 * **`not_allowed_unregistered_streams`**: This is sent back to the user if the publisher wants to send a stream with an unregistered id and server is configured not to allow this kind of streams.

 * **`no_room_specified`**: This is sent back to the user when there is no room specified in  joining the video conference.

 * **`unauthorized_access`**: This is sent back to the user when the token is not validated.

 * **`no_encoder_settings`**: This is sent back to the user when there are no encoder settings available in publishing the stream.

 * **`no_peer_associated_before`**: This is peer to peer connection error definition.It is sent back to the user when there is no peer associated with the stream.

 * **`notSetLocalDescription`**: It is send when local description is not set successfully.



#### Sample
Please take a look at the WebRTCAppEE/player.html file in order to see How JavaScript SDK can be used for playing a stream