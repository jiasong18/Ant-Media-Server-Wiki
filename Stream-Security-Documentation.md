This guide explains stream security options in Ant Media Server. Briefly, Stream Security options are; 
1. [Enable/Disable Accepting Undefined Streams](#1-enabledisable-accepting-undefined-streams)
2. [One Time Token Control](#2-one-time-token-control)
3. [CORS Filter](#3-cors-filter) 
4. [Hash-Based Token](#4-hash-based-token)
5. [Publisher IP Filter](#5-publisher-ip-filter)

## 1. Enable/Disable Accepting Undefined Streams

This setting shortly is checking if live stream is registered in Ant Media Server.

For example: If Ant Media Server accepts undefined streams, it accepts any incoming streams. If accepting undefined Streams is disabled, only streams with their stream id in the database are being accepted by Ant Media Server.

You can find in more detail in [here](Application-Configuration-Documentation)
<!-- > TODO: Please give the new configuration file. Not the old one. -->
After modifying the configuration, please add the streamId, stream name in "broadcast" collections of your App.

<!-- > TODO: Please also show the related web panel screen for this setting -->

![Accepting Undefined Streams](https://antmedia.io/wp-content/uploads/2019/12/antmedia-dashboard-accept-undefined-streams.png)

## 2. One Time Token Control 

One Time Token Control feature usage is in Dashboard / Application(LiveApp or etc.) / Publish/Play with One-time Tokens section.

<!-- > TODO: Please make screenshot has a larger view. It's not being understandable where it's -->

![One Time Token](https://antmedia.io/wp-content/uploads/2019/12/antmedia-dashboard-publish-one-time-token.png)

By enabling this option, one-time tokens are required for publishing and playing. Publish/Play requests without tokens will not be streamed.

If One-Time Token control is enabled, then all publish and play requests should be sent with a token parameter.

**Create a Token in Publish&Play Scenario**

The Server creates tokens with [getTokenV2](https://github.com/ant-media/Ant-Media-Server/blob/master/src/main/java/io/antmedia/rest/BroadcastRestService.java) Rest Service getting `streamId`, `expireDate` and `type` parameters with query parameters. Service returns `tokenId` and other parameters. It is important that `streamId` and type parameters should be defined properly. Because `tokenId` needs to match with both `streamId` and type.

The sample token creation service URL in Publish Scenario:

    http://[IP_Address]:5080/<Application_Name>/rest/v2/broadcasts/<Stream_Id>/token?expireDate=<Expire_Date>&type=publish

The sample token creation service URL in Play Scenario:

    http://[IP_Address]:5080/<Application_Name>/rest/v2/broadcasts/<Stream_Id>/token?expireDate=<Expire_Date>&type=play

Expire Date format is Unix Timestamp. Check also -> https://www.epochconverter.com/

**RTMP URL usage:**

    rtmp://[IP_Address]/<Application_Name>/streamID?token=tokenId

**Live Stream / VoD URL usage:**

    http://[IP_Address]/<Application_Name>/streams/streamID.mp4?token=tokenId
    http://[IP_Address]/<Application_Name>/streams/streamID.m3u8?token=tokenId
    http://[IP_Address]/<Application_Name>/play.html?name=streamID&playOrder=hls&token=tokenId
**WebRTC usage:**

**-Playing usage:** Again the token parameter should be inserted to play WebSocket message. Also please have a look at the principles described in the [WebRTC playing wiki page](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-WebSocket-Messaging-Details#playing-webrtc-stream).

<!--> TODO: Please tell or give link how to get token from Ant Media Server -->
<!-- > TODO: Please give the JavaScript method for that. Don't give from reference -->

    Secure WebSocket: wss://SERVER_NAME:5443/WebRTCAppEE/websocket
    WebSocket without Secure: ws://SERVER_NAME:5080/WebRTCAppEE/websocket

    {
    command : "play",
    streamId : "stream1",
    token : "tokenId",
    }

**-Publishing usage:** Again the token parameter should be inserted to WebSocket message. Also please have a look at the principles described in the [WebRTC publishing wiki page](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-WebSocket-Messaging-Details#publishing-webrtc-stream).

<!-- > TODO: Please tell or give link how to get token from Ant Media Server
> TODO: Please give the JavaScript method for that. Don't give from reference
-->
    Secure WebSocket: wss://SERVER_NAME:5443/WebRTCAppEE/websocket
    WebSocket without Secure: ws://SERVER_NAME:5080/WebRTCAppEE/websocket

    {
    command : "publish",
    streamId : "stream1",
    token : "tokenId",
    }

## 3. CORS Filter

CORS(Cross-Origin Resource Sharing) Filter is enabled and accepts requests from everywhere by default.

<!-- > TODO: It does not make sense to remove CORS filter. It should be customized.  -->

If you want to customize by yourself CORS Filters in Application, you can access in `SERVER_FOLDER` / `webapps` / `{Application}` / `WEB-INF` / web.xml

<!-- > TODO: Don't give image, please copy paste the code. 
![CORS Filter in Application](https://antmedia.io/wp-content/uploads/2019/03/CORS-Filter-in-Application.png)
-->

	<filter>
		<filter-name>CorsFilter</filter-name>
		<filter-class>io.antmedia.filter.CorsHeaderFilter</filter-class>
		<init-param>
		    <param-name>cors.allowed.origins</param-name>
		    <param-value>*</param-value>
		 </init-param>
		 <init-param>
		 	<param-name>cors.allowed.methods</param-name>
		 	<param-value>GET,POST,HEAD,OPTIONS,PUT,DELETE</param-value>
		 </init-param>
	</filter>
	<filter-mapping>
		<filter-name>CorsFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

If you want to customize by yourself CORS Filters in Root, you can access in `SERVER_FOLDER` / `webapps` / `root` / `WEB-INF` / web.xml

	<filter>
		<filter-name>CorsFilter</filter-name>
		<filter-class>io.antmedia.filter.CorsHeaderFilter</filter-class>
		<init-param>
		  <param-name>cors.allowed.origins</param-name>
		  <param-value>*</param-value>
		</init-param>
		<init-param>
		  <param-name>cors.allowed.methods</param-name>
		  <param-value>GET,POST,HEAD,OPTIONS,PUT,DELETE</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CorsFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

<!-- > If you remove CORS Filter, everyone can use your resources (m3u8, mp4 or etc) files and URL’s
> TODO: This is not true. If there is no CORS filter, browsers does not make request via ajax.  
-->

> Quick Learn: [Tomcat CORS Filter](https://tomcat.apache.org/tomcat-8.0-doc/api/index.html?org/apache/catalina/filters/CorsFilter.html)

## 4. Hash-Based Token

Firstly, settings should be enabled from the settings file of the application in `SERVER_FOLDER` / `webapp` / `{Application}` / `WEB-INF` / `red5-web.properties` 

<!-- > TODO: Please give settings file path of the application. -->

    settings.hashControlPublishEnabled=true
    settings.hashControlPlayEnabled=true
    tokenHashSecret=PLEASE_WRITE_YOUR_SECRET_KEY

Set true `settings.hashControlPublishEnabled` to enable secret based hash control for publishing operations, and `settings.hashControlPlayEnabled` for playing operations.

> Also, do not forget to define a secret key for generating a hash value.

### Publishing Scenario

**Step 1. Generate a Hash**

You need to generate a hash value using the formula `sha256(STREAM_ID+ROLE+SECRET)` for your application and send to your clients.  The values used for hash generation are:

    STREAM_ID: The id of stream, generated in Ant Media Server.
    ROLE: It is either "play or "publish"
    SECRET: Shared secret key (should be defined in the setting file)

**Step 2. Request with Hash**

The system controls hash validity during publishing or playing. __Keep in mind that there is NO '+' in calculating the hash in this formula `sha256(STREAM_ID+ROLE+SECRET)`__
Here is an example for that. 

Let's say `STREAM_ID: stream1`, `ROLE: publish`, `SECRET: this_is_secret`
Your hash is the result of this calculation: `sha256(stream1publishthis_is_secret)`

Go to [JavaScript SHA-256](https://geraintluff.github.io/sha256/) for online demo

**RTMP Publishing:** You need to add a hash parameter to RTMP URL before publishing. Sample URL:

    rtmp://[IP_Address]/<Application_Name>/<Stream_Id>?token=hash

**WebRTC Publishing:** Hash parameter should be inserted to publish WebSocket messages.

    {
    command : "publish",
    streamId : "stream1",
    token : "hash",
    }

### Playing Scenario 

**Step 1. Generate a Hash**

You need to generate a hash value using the formula sha256(STREAM_ID + ROLE + SECRET) for your application and send to your clients. The values used for hash generation are:

    STREAM_ID: The id of stream, generated in Ant Media Server.
    ROLE: It is either "play or "publish"
    SECRET: Shared secret key (should be defined in the setting file)

**Step 2. Request with Hash**

**Live Stream/VoD Playing:** Same as publishing, the hash parameter is added to the URL. Sample URL:

    http://[IP_Address]/<Application_Name>/streams/<Stream_Id_or_Source_Name>?token=hash

**WebRTC Playing:** Again the hash parameter should be inserted to play WebSocket message.

    {
    command : "play",
    streamId : "stream1",
    token : "hash",
    }

> Please have a look at the principles described in the [WebRTC WebSocket wiki page](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-WebSocket-Messaging-Reference).

### Evaluation of the Hash

If related settings are enabled, Ant Media Server first generates hash values based on the formula sha256(STREAM_ID + ROLE + SECRET) using streamId, role parameters and secret string which is defined in the settings file.

Then compare this generated hash value with the client's hash value during authentication.

Once the hash is successfully validated by Ant Media Server, the client is granted either to publish or play according to application setting and user request.

## 5. Publisher IP Filter

> Publisher IP Filter feature is available for later versions of the 1.9.0+ version.

Publisher IP filter feature allows you to specify the IP addresses allowed for publishing. You can define multiple allowed IPs in CIDR format as comma (,) separated.

To enable publisher IP filtering you must set `settings.allowedPublisherIps` in `AMS_DIR/webapps/<App_Name>/WEB_INF/red5.properties` file with the allowed IP addresses.

    Example: settings.allowedPublisherIps=10.20.30.40/24,127.0.0.1/32 allows IPs 10.20.30.[0-255] and 127.0.0.1.

You can [read more](https://whatismyipaddress.com/cidr/) about CIDR notation.