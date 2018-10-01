This documentation is for developers who need to error codes and their descriptions for WebRTC communication.

## Communication Callbacks

```json
{
    error : "notSetLocalDescription",
    description : " Error definition it is send when local description is not set successfully",
}
```
```json
{
    message : "takeConfiguration",
    description : "it is send when configuration is requested",
}
```
```json
{
    message : "takeCandidate",
    description : "it is send when iceCandidate is requested",
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
