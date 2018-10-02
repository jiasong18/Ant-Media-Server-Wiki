This documentation is for developers who need to callbacks and their descriptions for WebRTC operations.

## WebSocket Communication Callbacks

```json
{
    error : "notSetLocalDescription",
}
```
Description : it is send when local description is not set successfully",
## JS Error Callbacks 

**`"WebSocketNotSupported"`** Reason : WebSocket connection is not supproted for environement or connection is not in correct state.

**`"AbortError"`** Reason : Although the user and operating system both granted access to the hardware device, and no hardware issues occurred that would cause a NotReadableError, some problem occurred which prevented the device from being used.

**`"NotAllowedError"`** Reason : The user has specified that the current browsing instance is not permitted access to the device; or the user has denied access for the current session; or the user has denied all access to user media devices globally.

**`"NotFoundError"`** Reason : No media tracks of the type specified were found that satisfy the given constraints.

**`"OverconstrainedError"`** Reason : The specified constraints resulted in no candidate devices which met the criteria requested. The error is an object of type OverconstrainedError, and has a constraint property whose string value is the name of a constraint which was impossible to meet, and a message property containing a human-readable string explaining the problem.

**`"SecurityError"`** Reason : User media support is disabled on the Document on which getUserMedia() was called. The mechanism by which user media support is enabled and disabled is left up to the individual user agent.

**`"TypeError"`** Reason : The list of constraints specified is empty, or has all constraints set to false.

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





