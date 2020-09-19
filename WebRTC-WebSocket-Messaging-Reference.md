This documentation is for developers who needs to implement signalling between Ant Media Server and clients 
for publishing & playing streams. Let's make it step by step

## Publishing WebRTC Stream

1. Client connects to Ant Media Server through WebSocket. URL of the WebSocket interface is something like

```
ws://SERVER_NAME:5080/WebRTCAppEE/websocket
```

2. Client sends publish JSON command to the server with stream name parameter. (Remove token parameter if token control is not enabled) 

```yml
{
    command : "publish",
    streamId : "stream1",
    token : "tokenId",
}
```

3. If Server accepts the stream, it replies back with start command
```yml
{
    command : "start",
    streamId : "stream1",
}
```

4. Client inits peer connections, creates offer sdp and send the sdp configuration 
to the server with takeConfiguration command
```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "offer",  
   sdp : "${SDP_PARAMETER}"
}
```

5. Server creates answer sdp and send the sdp configuration to the client with takeConfiguration command
```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "answer",  
   sdp : "${SDP_PARAMETER}"
}
```
6. Client and Server get ice candidates several times and sends to each other with takeCandidate command
```yml
{
    command : "takeCandidate",
    streamId : "stream1",
    label : "${CANDIDATE.SDP_MLINE_INDEX}",
    id : "${CANDIDATE.SDP_MID}",
    candidate : "${CANDIDATE.CANDIDATE}"
}

```

7. Clients sends stop JSON command to stop publishing
```yml
{
    command : "stop",
    streamId: "stream1"
}
```

## Playing WebRTC Stream

1. Client connects to Ant Media Server through WebSocket.

```
ws://SERVER_NAME:5080/WebRTCAppEE/websocket
```

2. Client sends play JSON command to the server with stream name parameter. (Remove token parameter if token control is not enabled) 

```yml
{
    command : "play",
    streamId : "stream1",
    token : "tokenId",

}
```

3. If Server accepts the stream, it replies back with offer command
```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "offer",  
   sdp : "${SDP_PARAMETER}"
}
```

5. Client creates answer sdp and send the sdp configuration to the server with takeConfiguration command
```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "answer",  
   sdp : "${SDP_PARAMETER}"
}
```
6. Client and Server get ice candidates several times and sends to each other with takeCandidate command
```yml
{
    command : "takeCandidate",
    streamId : "stream1",
    label : "${CANDIDATE.SDP_MLINE_INDEX}",
    id : "${CANDIDATE.SDP_MID}",
    candidate : "${CANDIDATE.CANDIDATE}"
}

```

7. Clients sends stop JSON command to stop playing
```yml
{
    command : "stop",
    streamId: "stream1",
}
```


## Peer to Peer WebRTC Stream

1. Peers connects to Ant Media Server through WebSocket.

```
ws://SERVER_NAME:5080/WebRTCAppEE/websocket
```

2. Client sends join JSON command to the server with stream name parameter. 

```yml
{
    command : "join",
    streamId : "stream1",
}
```

If there is only one peer in the stream1, server waits for the other peer to join the room. 

3. When second peer joins the stream, server sends start JSON command to the first peer 

```yml
{
    command : "start",
    streamId : "stream1",
}
```

4. First peer create offer sdp and send to the server with takeConfiguration command, 

```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "offer",  
   sdp : "${SDP_PARAMETER}"
}
```
Server relays the offer sdp to the second peer

5. Second peer creates answer sdp and sends to the server with takeConfiguration command
```yml
{
   command : "takeConfiguration",
   streamId : "stream1",
   type : "answer",  
   sdp : "${SDP_PARAMETER}"
}
```
Server relays the answer sdp to the first peer

6. Each peers get ice candidates several times and sends to each other with takeCandidate command through server
```yml
{
    command : "takeCandidate",
    streamId : "stream1",
    label : "${CANDIDATE.SDP_MLINE_INDEX}",
    id : "${CANDIDATE.SDP_MID}",
    candidate : "${CANDIDATE.CANDIDATE}"
}

```

7. Clients sends leave JSON command to leave the room
```yml
{
    command : "leave",
    streamId: "stream1"
}
```

## Conference WebRTC Stream

1. Peers connects to Ant Media Server through WebSocket.

```
ws://SERVER_NAME:5080/WebRTCAppEE/websocket
```

2. Client sends join JSON command to the server with room name parameter. `streamId` field is optional in case `streamId` should be specified in advance. If `streamId` is not sent, server returns with a random `streamId` in the second message.

```yml
{
    command : "joinRoom",
    room : "room_id_for_your_conference",
    streamId: "stream_id_you_want_to_use"
}
```
3. Server notifies the client with available streams in the room
```yml
{
    command : "notification",
    definition : "joinedTheRoom",
    streamId: "unique_stream_id_returned_by_the_server"
    streams: [
        "stream1_in_the_room",
        "stream2_in_the_room",
        ...
    ]
}
```
```streamId``` returned by the server is the stream id client uses to publish stream to the room. 
```streams``` is the json array which client can play via WebRTC. Client can play each stream by play method above. This streams array can be empty if there is no stream in the room.

4. Web app should pull the server periodically for the room info as follows, 
```yml
{
    command : "getRoomInfo",
    room : "room_id_for_your_conference",
    streamId: "unique_stream_id_returned_by_the_server"
}
```
5. Server returns the active streams in the room as follows. Application should synchronize the players in their side.

```yml
{
   command:"roomInformation",
   room: "room_id_for_your_conference",
   streams: [
              "stream1_in_the_room",
              "stream2_in_the_room",
              ...
            ]
}
```

6. Any user can leave the room by sending below message
```yml
{
    command : "leaveFromRoom",
    room: "roomName"
}
```
## WebSocket Error Callbacks

  * `noStreamNameSpecified`: it is sent when stream id is not specified in the message.

```yml
{
    command : "error",
    definition : "noStreamNameSpecified",
}
```
  * `not_allowed_unregistered_streams`: This is sent back to the user if the publisher wants to send a stream with an unregistered id and server is configured not to allow this kind of streams
```yml
{
    command : "error",
    definition: "not_allowed_unregistered_streams",
}
```

  * `no_room_specified`: This is sent back to the user when there is no room specified in  joining the video conference.
```yml
{
    command : "error",
    definition : "no_room_specified",
}
```

  * `unauthorized_access`:This is sent back to the user when the token is not validated
```yml
{
    command : "error",
    definition : "unauthorized_access",
}
```

  * `no_encoder_settings`:This is sent back to the user when there are no encoder settings available in publishing the stream.
```yml
{
    command : "error",
    definition : "no_encoder_settings",
}
```

  * `no_peer_associated_before`: This is peer to peer connection error definition.It is sent back to the user when there is no peer associated with the stream.
```yml
{
    command : "error",
    definition : "no_peer_associated_before",
}
```
  * `notSetLocalDescription`: It is sent when local description is not set successfully
```yml
{
    command : "error",
    definition : "notSetLocalDescription",
}
```
  * `highResourceUsage`: It is sent when server is overloaded. Server overload means over CPU usage or over RAM usage. Over CPU usage means CPU load is more than the `server.cpu_limit` value  in `conf/red5.properties`. Its default value is %85. Over RAM usage means available memory in the system is less than `server.min_free_ram` value in `conf/red5.properties`. Its unit is MB and default value is 10.
  ```yml
  {
    command : "error",
    definition : "highResourceUsage",
  }
  ```
  * `streamIdInUse`: The server sends this message if it detects that there is already an active stream(preparing or publishing state) with that same stream id when a user tries to publish a stream. One may get this error if s/he tries to re-publish a stream with the same stream id without closing the previous WebRTC connection. 
  ```yml
  {
    command : "error",
    definition : "streamIdInUse",
  }
  ```
  * `publishTimeoutError`: The server sends this message if WebRTC publishing is not started in a specified time period. This value is configurable via `settings.webrtc.client.start.timeoutMs` property in App Configuration. Its default value is 5000 milliseconds   
  ```yml
  {
    command : "error",
    definition : "publishTimeoutError",
  }
  ```

## Miscellaneous WebSocket Methods
* `ping` & `pong` 
  Whenever you send a ping command to the server, it will respond you with pong command. 
  Ping Command
  ```yml
  {
    command : "ping",
  }
  ```

  Pong Response from Server
  ```yml
  {
    command : "pong",
  }
  ```
* `getStreamInfo`: Get Stream Information from Server. You may use this method to learn more about stream status and bitrates. Client should send the following message.
  ```yml
  {
     command: "getStreamInfo",
     streamId: "stream_id_that_you_want_to_get_info"
  } 
  ```
  Server returns in two ways. It may return stream information as follows
  ```yml
  {
     command: "streamInformation",
     streamId: "stream_id_of_the_stream_information",
     streamInfo: [{
                   streamWidth: resolution_width,
                   streamHeight: resolution_height,
                   videoBitrate: video_bitrate,
                   audioBitrate: audio_bitrate,
                   videoCodec: codec_of_the_video 
                 },
                 ...
                 ]
  }
  ```

  If stream is not active, it will return `no_stream_exist`
  ```yml
  {
    command : "error",
    definition : "no_stream_exist",
    streamId: "id_of_the_stream"
  }
  ```
* `getRoomInfo`: Get Room Information from server that returns the whole active streams in the room. Client should send the following message to get the response from the server.
  ```yml
  {
     command: "getRoomInfo",
     room: "room_id_that_you_want_to_get_info",
     streamId: "server_returns_while_you_join_the_room"
  } 
  ```
  Server responds in following format
  ```yml
  {
     command: "roomInformation",
     room: "room_id_that_this_information_belongs_to",
     streams: [ stream_id_1, stream_id_2, ...]
  }
  ```
* `bitrateMeasurement`: Server periodically sends this information to the WebRTC viewers. It lets develop show a message to the user if it's internet bandwidth is not good enough. If the `targetBitrate` is bigger than the sum of `videoBitrate` and `audioBitrate`, it means internet bandwidth is good enough to play the video. If the `targetBitrate`  is less than the sum of `videoBitrate` and `audioBitrate`, it means some playback issues(pixelating, packet drop, etc.) may happen and it disturbs the user experience. 
  ```yml
  {
    command : "notification",
    definition : "bitrateMeasurement",
    streamId: "unique_stream_id_returned_by_the_server"
    targetBitrate: measured_bandwidth_of_the_client,
    videoBitrate: video_bitrate_of_the_current_playing_video,
    audioBitrate: "audio_bitrate_of_the_current_playing_audio
  }
  ```
* `forceStreamQuality`: If there are adaptive-bitrates(multi-bitrate) for that stream, you can get bitrates with `getStreamInfo` method and then you can make the Ant Media Server force to send you a specific resolution. 
  ```yml
  {
    command: "forceStreamQuality",
    streamId: "write_the_stream_id",
    streamHeight: write_the_height_of_the_resolution_you_want_to_force
  }
  ```