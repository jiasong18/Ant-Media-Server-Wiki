You have a streaming project or just want to stream personally, however, may have some concerns about its reachability. The one-time token method is one of the effective authentication methods to secure your streams. Ant Media Server offers one-time token security control option with 1.5.0 version.

### The Parameters of the Token

**TokenId:** Generated a random string from service.

**StreamId:** The Id of the resource that user wants to reach.

**ExpireDate:** The expiration date of the token.

**Type:** Either publish or play token.

## The Steps for Token Control Mechanism

### Step 1. Enable Setting

Firstly, the setting should be enabled in the management panel.

![token setting](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/09/Screenshot-from-2018-09-17-20-53-01-1024x441.png)

### Step 2. Create a Token

The Server creates tokens with [getToken](https://github.com/ant-media/Ant-Media-Server/blob/14e243dd8f1696fbbc66b582eadbbe301e516e72/src/main/java/io/antmedia/rest/BroadcastRestService.java#L976) Rest Service getting streamId, expireDate and type parameters with query parameters. Service returns tokenId and other parameters. It is important that streamId and type parameters should be defined properly. Because tokenId needs to match with both streamId and type.

### Step 3. Request with Token 

The system controls token validity during publishing or playing.

**a) Publishing** 

**RTMP Publishing:** You need to add a token parameter to RTMP URL before publishing. Sample URL:

`rtmp://[IP_Address]/<Application_Name>/ 312526128013151313200552?token=tokenId`

**WebRTC Publishing:** Token parameter should be inserted to publish WebSocket message.
```json
{
    command : "publish",
    streamId : "stream1",
    token : "tokenId",
}
```

 For details about WebRTC WebSocket messaging please visit [wiki page](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-WebSocket-Messaging-Details).

**b) Playing/Accessing**

**Live Stream/VoD Playing:** Same as publishing, the token parameter is added to URL. Sample URL:

``http://[IP_Address]/<Application_Name>/streams/250116815996644357614115.mp4?token=tokenId``