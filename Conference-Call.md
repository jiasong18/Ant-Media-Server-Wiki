<!-- > Add below subtitles
> 1. How to join room
> 2. How to publish room
> 3. How to play streams in the room
>   * Server notifications about new stream joined/leaved
-->

In this documentation, we're going to explain simply how to implement WebRTC Video Conference call with JavaScript SDK. There is already a working demo for this. You can check it out in advance.
````
https://domain-name.com:5443/LiveApp/conference.html
````
File is located `/usr/local/antmedia/webapps/LiveApp/conference.html`

> WebRTC Video Conference is available in Enterprise Edition. 
 
Let’s proceed step by step about how to implement Conference Call. Before starting implementation, make sure that you've installed SSL to your Ant Media Server Enterprise Edition. If you haven’t got any domain for your Ant Media Server, you can get a free domain in [Freenom](https://www.freenom.com/).

> Quick Link: [Learn How to Install SSL to your Ant Media Server](SSL-Setup) 

### 1. Prepare the Web Page
Please go to `LiveApp` application folder which is under `/usr/local/antmedia/webapps/LiveApp` and create 
a file with `conference_call.html`.

<img src="https://antmedia.io/wp-content/uploads/2019/12/ant-media-server-conference.jpg" alt="Conference Room Example" align="center" />

* Include JavaScript files to your page in the header as follows

```javascript
<head>
...
<script src="https://webrtc.github.io/adapter/adapter-latest.js"></script>
<script src="js/webrtc_adaptor.js"></script>
...
</head>
```

* Include Codes to your page in the body as follows.

```javascript
<body>
...
<div class="jumbotron">
<div id="players">
<div class="col-sm-3">
<video id="localVideo" autoplay muted controls playsinline></video>
</div>
</div>

<div class="row">
<div class="col-sm-6 col-sm-offset-3">
<p>
<input type="text" class="form-control" value="room1" id="roomName" placeholder="Type room name">
</p>
<p>
<button onclick="joinRoom()" class="btn btn-info" disabled id="join_publish_button">Join Room</button>
<button onclick="leaveRoom()" class="btn btn-info" disabled id="stop_publish_button">Leave Room</button>
</p>

<div style="padding:10px">
<button id="turn_off_camera_button" onclick="turnOffLocalCamera()" class="btn btn-default"  >Turn off Camera</button>
<button id="turn_on_camera_button" disabled onclick="turnOnLocalCamera()" class="btn btn-default"  >Turn on Camera</button>

<button id="mute_mic_button" onclick="muteLocalMic()" class="btn btn-default"  >Mute Local Mic</button>
<button id="unmute_mic_button" disabled onclick="unmuteLocalMic()" class="btn btn-default"  >Unmute Local Mic</button>
</div>
<span class="label label-success" id="broadcastingInfo" style="font-size: 14px; display: none" style="display: none">Publishing</span>
</div>
</div>
</div>
...
</body>
```

* Include the JavaScript codes to your page. Please take a look at the full JavaScript code at [conference.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/conference.html). We explain the callbacks below in order to provide better understanding.

```javascript
<script>
...

var webRTCAdaptor = new WebRTCAdaptor(
		{
			websocket_url : websocketURL,
			mediaConstraints : mediaConstraints,
			peerconnection_config : pc_config,
			sdp_constraints : sdpConstraints,
			localVideoId : "localVideo",
			isPlayMode : playOnly,
			debug : true,
			callback : function(info, obj) {
				if (info == "initialized") {
				    //called by JavaScript SDK when WebSocket is connected. 
				} else if (info == "joinedTheRoom") {
			          //called when this client is joined the room
	                          //obj contains streamId field which is the stream id
                                  //that this client can use to publish to the room.
                                  //after client sending joinedTheRoom command, server replies back with the streams in the room.
                                  //so that you can play these streams in here
                                  //periodically calls getRoominfo to checks for any new stream joined to the room
				} else if (info == "newStreamAvailable") {
                                    //called when server gets a new stream while checking periodically for new streams.
                                    //if there is a new stream, plays it.
				} else if (info == "publish_started") {
				    //called when stream publishing is started for this client		
				} else if (info == "publish_finished") {
				    //called when stream publishing has finished for this client
				} else if (info == "leavedFromRoom") {
			            //called when this client is leaved from the room  	
				} else if (info == "closed") {
				    //called when websocket connection is closed	
				} else if (info == "play_finished") {
				    //called when a stream has finished playing	
				} else if (info == "streamInformation") {
				    //called when a stream information is received from the server		
				} else if (info == "roomInformation") {
				    //gets from the room information from the server when getRoomInfo is called.
                                    //returns the array of streams.
				
				}else if (info == "data_channel_opened") {
				    //called when data channel is opened
				} else if (info == "data_channel_closed") {
				    // called when data channel is closed
				} else if(info == "data_received") {
                                   //called when data is received through data channel
				}
			},
			callbackError : function(error, message) {
		//some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError
	  }
});
...
</script>
```
### 2. Join a Room

When WebRTCAdaptor is initialized successfully, it creates a websocket connection. After a successful connection, the client gets the `initialized` notification from the server. After receiving `initialized`
notification, you can call `joinRoom` method.
````
webRTCAdaptor.joinRoom(roomId , streamId);
````
`joinRoom` method gets two parameters
* `roomId`:(Mandatory) The room id. If this room is not available, it will be created. 
* `streamId`:(Optional) The stream id of the stream that this client would want to publish  

### 3. Publish Stream to a Room

Just call publish method as follows. 
````
webRTCAdaptor.publish(streamId, token);
````
`publish` method gets two parameters:
*`streamId`:(Mandatory) Stream id of the stream.
* `token`: It's required only when one-time token is enabled. 


### 4. Play Stream in a Room

Call the play method. In conference call the correct place to call method is when `joinedTheRoom` and `streamJoined` notifications are received. 
```
webRTCAdaptor.play(streamId, token);
```
`play` method gets two parameters:
*`streamId`:(Mandatory) Stream id of the stream.
* `token`: It's required only when one-time token is enabled. 


### 5. Server Notifications 
Here are the conference related notifications that callback is invoked for. Please check the [conference.html](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/conference.html) for proper usage. 
* `joinedTheRoom`: Called when WebSocket is connected. It has parameter that contains stream id for publishing and streams array in order to play the stream.   
* `streamJoined`:  Called when a new stream is joined to the room. It has parameter about the stream id for playing. 
* `newStreamAvailable`: Called when a previously joined stream is ready to play. 
* `streamLeaved`: Called when a stream is leaved from the room. 
* `leavedFromRoom`: Called when this client is leaved from the room. 
