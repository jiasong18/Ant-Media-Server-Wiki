This documentation is for developers who need to callbacks and their descriptions for WebRTC operations.

## Communication Callbacks

```json
{
    error : "notSetLocalDescription",
}
```
 description : " Error definition it is send when local description is not set successfully",
## JS Callbacks

WebSocketNotSupported -  !("WebSocket" in window)
WebSocketNotSupported - (wsConn.readyState == 0 || wsConn.readyState == 2 || wsConn.readyState == 3) 

callbackError - navigator.mediaDevices.getUserMedia(thiz.mediaConstraints)
callbackError - navigator.mediaDevices.getUserMedia({audio:true, video:false}).then(function(micStream)
callbackError - (typeof thiz.mediaConstraints.audio != "undefined" && thiz.mediaConstraints.audio != false)
callbackError - var media_audio_constraint = { audio: thiz.mediaConstraints.audio }
callbackError - if (typeof thiz.mediaConstraints.video != "undefined" && thiz.mediaConstraints.video != "false")
callbackError - navigator.mediaDevices.getUserMedia({audio:thiz.mediaConstraints.audio})

```json
{
    message : "AudioAlreadyActive",
    description : " if there is audio it calls callbackError with "AudioAlreadyActive" ,
}
```
```json
{
    error : "Camera or Mic is being used by some other process that does not let read the devices",
    description : " Error definition it is send when media devices are used by another applications",
}
```
```json
{
    message : "VideoAlreadyActive",
    description : " if there is video it calls callbackError with "VideoAlreadyActive",
}
```
```json
{
    error : "NotSupportedError",
    description : " Error definition it is send when SSL is needed",
}
```
```json
{
    error : "NoActiveConnection",
    description : " Error definition it is send when no active WebSocket connection",
}
```

## Other Callbacks

```json
{
    error : "noStreamNameSpecified",
    description : " Error definition it is send when stream id is not specified in the message",
}
```
```json
{
    error : "not_allowed_unregistered_streams",
    description : " This is sent back to the user if publisher wants to send a stream with an unregistered id 
                       and server is configured not to allow this kind of streams",
}
```
```json
{
    error : "no_room_specified",
    description : " This is sent back to the user when there is no room specified in  joining the video 
                conference",
}
```
```json
{
    error : "unauthorized_access",
    description : " This is sent back to the user when token is not validated",
}
```
```json
{
    error : "no_encoder_settings",
    description : " This is sent back to the user when there is no encoder settings available
	     in publishing the stream",
}
```
```json
{
    error : "no_peer_associated_before",
    description : " This is peer to peer connection error definition.
	       It is sent back to the user when there is no peer associated with the stream",
}
```





