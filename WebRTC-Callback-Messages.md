This documentation is for developers who need to callbacks and their descriptions for WebRTC operations.

## WebSocket Communication Callbacks

```json
{
    error : "notSetLocalDescription",
}
```
Description : it is send when local description is not set successfully",
## JavaScript Error Callbacks 

**`"WebSocketNotSupported"`** Reason: WebSocket connection is not supported for environment or connection is not in the correct state.

**`"AbortError"`** Reason: Although the user and operating system both granted access to the hardware device, and no hardware issues occurred that would cause a NotReadableError, some problem occurred which prevented the device from being used.

**`"NotAllowedError"`** Reason: The user has specified that the current browsing instance is not permitted access to the device, or the user has denied access for the current session, or the user has denied all access to user media devices globally.

**`"NotFoundError"`** Reason: No media tracks of the type specified were found that satisfy the given constraints.

**`"OverconstrainedError"`** Reason: The specified constraints resulted in no candidate devices which met the criteria requested. The error is an object of type OverconstrainedError and has a constraint property whose string value is the name of a constraint which was impossible to meet, and a message property containing a human-readable string explaining the problem.

**`"SecurityError"`** Reason: User media support is disabled on the Document on which getUserMedia() was called. The mechanism by which user media support is enabled and disabled is left up to the individual user agent.

**`"AudioAlreadyActive"`** Reason: If there is audio it calls callbackError with "AudioAlreadyActive.

**`"Camera or Mic is being used by some other process that does not let read the devices"`** Reason: Error definition it is sent when media devices are used by another application.

**`"VideoAlreadyActive"`** Reason: If there is video it calls callbackError with "VideoAlreadyActive.

**`"NotSupportedError"`** Reason: Error definition it is sent when SSL is needed.

**`"NoActiveConnection"`** Reason: Error definition it is sent when no active WebSocket connection.


## WebSocket Error Callbacks

```json
{
    error : "noStreamNameSpecified",
}
```
Description: Error definition it is sent when stream id is not specified in the message.

```json
{
    error: "not_allowed_unregistered_streams",
}
```
Description: This is sent back to the user if the publisher wants to send a stream with an unregistered id and server is configured not to allow this kind of streams",

```json
{
    error : "no_room_specified",
}
```
Description: This is sent back to the user when there is no room specified in  joining the video conference.

```json
{
    error : "unauthorized_access",
}
```
Description : This is sent back to the user when the token is not validated",

```json
{
    error : "no_encoder_settings",
}
```
Description : This is sent back to the user when there are no encoder settings available in publishing the stream.

```json
{
    error : "no_peer_associated_before",
}
```
Description: This is peer to peer connection error definition.It is sent back to the user when there is no peer associated with the stream",




