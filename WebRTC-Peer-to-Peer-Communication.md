In this documentation, we're going to explain how to implement WebRTC Peer to Peer Communication with JavaScript SDK. There is already a working demo for this. You can check it out in advance.

````
https://domain-name.com:5443/LiveApp/peer.html
````
File is located `/usr/local/antmedia/webapps/LiveApp/peer.html`

> WebRTC Video Conference is available in Enterprise Edition. 

## Implementation of Peer to Peer Communication

Let’s proceed step by step for implementing Peer to Peer Call. Before starting implementation, make sure that you've installed SSL to your Ant Media Server Enterprise Edition. If you haven’t got any domain for your Ant Media Server, you can get a free domain in [Freenom](https://www.freenom.com/).

> Quick Link: [Learn How to Install SSL to your Ant Media Server](SSL-Setup) 

 
### 1. Prepare the Web Page
Please go to `LiveApp` application folder which is under `/usr/local/antmedia/webapps/LiveApp` and create 
a file with `peer.html`.

<img src="https://antmedia.io/wp-content/uploads/2019/12/ant-media-server-peer-to-peer.jpg" alt="WebRTC Peer to Peer Communication" align="center" />

Include JavaScript files to your page in the header as follows

```javascript
<head>
...
<script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
<script src="js/webrtc_adaptor.js"></script>
...
</head>
```

Include Codes to your page in the body as follows.

```javascript
<body>
...
<video id="localVideo" autoplay muted width="480"></video>
<video id="remoteVideo" autoplay controls width="480"></video>
<br /> <br />
<div class="input-group col-sm-offset-2 col-sm-8">
<input type="text" class="form-control" value="stream1" id="streamName" placeholder="Type stream name"> <span class="input-group-btn">
<button onclick="join()" class="btn btn-default" disabled id="join_button">Join</button>
<button onclick="leave()" class="btn btn-default" disabled id="leave_button">Leave</button>
</span>
</div>
<div style="padding:10px">
<button onclick="turnOffLocalCamera()" class="btn btn-default"  >Turn off Camera</button>
<button onclick="turnOnLocalCamera()" class="btn btn-default"  >Turn on Camera</button>

<button onclick="muteLocalMic()" class="btn btn-default"  >Mute Local Mic</button>
<button onclick="unmuteLocalMic()" class="btn btn-default"  >Unmute Local Mic</button>
</div>
...
</body>
```

* Include the JavaScript codes to your page. Please take a look at the full JavaScript code at [peer.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/peer.html). 

```javascript
<script>
...
Define Media Source variable, SDP variable and etc.

Define websocketURL your URL.
var websocketURL = "wss://domain-name.com:5443/WebRTCAppEE/websocket";

var webRTCAdaptor = new WebRTCAdaptor({
		  websocket_url: websocketURL,
		  mediaConstraints: mediaConstraints,
		  peerconnection_config: pc_config,
		  sdp_constraints: sdpConstraints,
		  localVideoId: "localVideo",
		  remoteVideoId: "remoteVideo",
		  callback: function(info) {
			  if (info == "initialized") {
				  console.log("initialized");
			  }
			  else if (info == "joined") {
				  //joined the stream
				  console.log("joined");
			  }
			  else if (info == "leaved") {
				  //leaved the stream
				  console.log("leaved");
			  }
		  },
		  callbackError: function(error) {
			  //some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError
			  
			  console.log("error callback: " + error);
			  alert(error);
		  }
	  });
	
...
</script>
```

### 2. Join a Peer to Peer Communication

When WebRTCAdaptor is initialized successfully, it creates a web socket connection. After a successful connection, the client gets the “initialized” notification from the server. After receiving `initialized`
notification, you can call `join` method.
````
webRTCAdaptor.join(streamId);
````
`join` method gets one parameter
* `streamId`:(Mandatory) The id of the peer to peer connection that this client would want to join  


If `join` method returns successful, the server responds with `joined` notification. As a result `callback``
method is called with `joined` notification.

### 3. Leave from a Peer to Peer Communication

When you want to leave from Peer to Peer connection, just call `leave` method. 
````
webRTCAdaptor.leave(streamId);
````
`leave` method gets one parameter
* `streamId`:(Mandatory) The id of the peer to peer connection that this client would want to leave from  


## Auxiliary Methods

JavaScript SDK provides several auxiliary methods to provide enough flexibility in your application. 

* `turnOffLocalCamera`: Turn off the local camera in WebRTC Peer to Peer Communication 
````
webRTCAdaptor.turnOffLocalCamera();
````
 
* `turnOnLocalCamera`: Turn on the local camera in WebRTC Peer to Peer Communication
````
webRTCAdaptor.turnOnLocalCamera();
````
* `muteLocalMic`: Mutes the local microphone in WebRTC Peer to Peer Communication
````
webRTCAdaptor.muteLocalMic();
````

* `unmuteLocalMic`: Unmute the local microphone in WebRTC Peer to Peer Communication
````
webRTCAdaptor.unmuteLocalMic();
````
### TURN Server

In some cases, Peer to Peer Communication cannot be established and a relay server is required for video/audio transmission. For this requirement, TURN servers are needed to relay the video/audio.   

![WebRTC DataPaths](https://github.com/mekya/antmedia-doc/blob/master/dataPathways.png?raw=true)

[Coturn](https://github.com/coturn/coturn) can be used as a TURN Server.
You can enter TURN Server credentials in [peer.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/peer.html) as follows.
````javascript
var pc_config =
  {
    'iceServers' : 
       [ {
         'urls' : 'turnServerURL',
         'username' : 'turnServerUsername',
         'credential' : 'turnServerCredential'
       } ]
  };
````
