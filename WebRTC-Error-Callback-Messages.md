This documentation is for developers who need to callbacks and their descriptions for WebRTC operations.


## JavaScript Error Callbacks 

  * **`WebSocketNotSupported`**: WebSocket connection is not supported for environment or connection is not in the correct state.

  * **`AbortError`**: Although the user and operating system both granted access to the hardware device, and no hardware issues occurred that would cause a NotReadableError, some problem occurred which prevented the device from being used.

 * **`NotAllowedError`**: The user has specified that the current browsing instance is not permitted access to the device, or the user has denied access for the current session, or the user has denied all access to user media devices globally.

 * **`NotFoundError`**: No media tracks of the type specified were found that satisfy the given constraints.

 * **`OverconstrainedError`**: The specified constraints resulted in no candidate devices which met the criteria requested. The error is an object of type OverconstrainedError and has a constraint property whose string value is the name of a constraint which was impossible to meet, and a message property containing a human-readable string explaining the problem.

 * **`SecurityError`**: User media support is disabled on the Document on which getUserMedia() was called. The mechanism by which user media support is enabled and disabled is left up to the individual user agent.

 * **`AudioAlreadyActive`**: If there is audio it calls callbackError with "AudioAlreadyActive.

 * **`Camera or Mic is being used by some other process that does not let read the devices`**: Error definition it is sent when media devices are used by another application.

 * **`VideoAlreadyActive`**: If there is video it calls callbackError with "VideoAlreadyActive.

 * **`NotSupportedError`**: Error definition it is sent when SSL is needed.

 * **`noStreamNameSpecified`**: Error definition it is sent when stream id is not specified in the message.

 * **`not_allowed_unregistered_streams`**: This is sent back to the user if the publisher wants to send a stream with an unregistered id and server is configured not to allow this kind of streams.

 * **`no_room_specified`**: This is sent back to the user when there is no room specified in  joining the video conference.

 * **`unauthorized_access`**: This is sent back to the user when the token is not validated.

 * **`no_encoder_settings`**: This is sent back to the user when there are no encoder settings available in publishing the stream.

 * **`no_peer_associated_before`**: This is peer to peer connection error definition.It is sent back to the user when there is no peer associated with the stream.

 * **`notSetLocalDescription`**: It is sent when local description is not set successfully.

 * **`screen_share_permission_denied`**: It is sent when user does not allow screen share

 * **`screen_share_stopped`**: It is sent when user clicks `Stop sharing` button in the screen sharing dialog of chrome

