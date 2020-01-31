The Ant Media Server configurations can be made directly from the files as well as through the management console. The configurations file is more detailed.

### Settings File Under Applications

These settings are set for each applications and stored in the file `<AMS_DIR>/webapps/<AppName>/WEB_INF/red5-web.properties`.

The table below summarises the available settings. Some settings don’t have to be in the file but some of them are mandatory. Mandatory settings are marked with. If you don’t add the settings in the file then AMS works with default values.

* **`settings.mp4MuxingEnabled`**: It's mandatory. If it is set `true` then a .mp4 file is created into `<APP_DIR>/streams` directory. Default value is `false`.
* **`settings.addDateTimeToMp4FileName`**: It's mandatory. Date and time are added to created .mp4 file name. Default value is `false`.
* **`settings.encoderSettingsString`**: This must be set for adaptive streaming. If it is empty SFU mode will be active in WebRTCAppEE. video height, video bitrate, and audio bitrate are set as an example. Ex. `480,300000,96000,360,200000,64000`.
* **`settings.hlsMuxingEnabled`**: If it is set `true` then HLS files are created into `<APP_DIR>/streams` and HLS playing is enabled. Default value is `true`.
* **`settings.hlsListSize`**: Set the maximum number of playlist entries. If 0 the list file will contain all the segments.
* **`settings.hlsTime`**: Target segment length in seconds. Segment will be cut on the next key frame after this time has passed.
* **`settings.deleteHLSFilesOnEnded`**: It's mandatory. If it is set then created segment files will be deleted after streaming. Default value is `true`.
* **`settings.hlsPlayListType`**: *event:* Forces hls_list_size to 0; the playlist can only be appended to. *vod:* Forces hls_list_size to 0; the playlist must not change.
* **`settings.webRTCEnabled`**: It's mandatory. If it is set true then WebRTC playing is enabled. Default value is `false`
* **`settings.listenerHookURL`**: You must set this to subscribe some event notifications. For details check: https://antmedia.io/webhook-integration/
* **`settings.acceptOnlyStreamsInDataStore`**: It's mandatory. If it is set true you cannot start publishing unless you add the stream id to the database. You can add stream id by REST API. Default value is `false`.
* **`settings.tokenControlEnabled`**: It's mandatory. This enables token control. Check for details: https://antmedia.io/secure-video-streaming/. Default value is `false`.
* **`tokenHashSecret`**: The key that used in hash generation for hash-based access control.
* **`settings.hashControlPublishEnabled`**: It's mandatory. If it is set true then hash based access control enabled for publishing. Default value is `false`.
* **`settings.hashControlPlayEnabled`**: It's mandatory. If it is set true then hash based access control enabled for playing. Default value is `false`.
* **`facebook.clientId`**: This is client id provided by Facebook to broadcast streams to Facebook.
* **`facebook.clientSecret`**: Secret key for the client id above.
* **`periscope.clientId`**: This is client id provided by Periscope to broadcast streams to Periscope.
* **`periscope.clientSecret`**: Secret key for the client id above.
* **`youtube.clientId`**: This is client id provided by YouTube to broadcast streams to YouTube.
* **`youtube.clientSecret`**: Secret key for the client id above.
* **`settings.vodFolder`**: Determines the directory to store VOD files.
* **`settings.stalkerDBServer`**: Database host address of IP TV Ministra platform.
* **`settings.stalkerDBUsername`**: Database user name of IP TV Ministra platform.
* **`settings.stalkerDBPassword`**: Database password of IP TV Ministra platform.
* **`settings.objectDetectionEnabled`**: It's mandatory. If it is set true then object detection algorithm is run for streaming video. Default value is `false`.
* **`settings.createPreviewPeriod`**: It's mandatory. This determines the period (milliseconds) of preview (png) file creation. This file is created into `<APP_DIR>/preview` directory. Default value is `5000`.
* **`settings.previewHeight`**: It's mandatory. Determines the height of preview file. Default value is `480`
* **`settings.previewOverwrite`**: If it is set `true` and new stream starts with the same id, preview of the new one overrides the previous file. If it is `false` previous file saved with a suffix.
* **`settings.streamFetcherBufferTime`**: It's mandatory. Buffering time for fetched streams from external sources. 0 means no buffer. Default value is `0`
* **`settings.streamFetcherRestartPeriod`**: It's mandatory. Restart time for fetched streams from external sources. Default value is `0`
* **`settings.muxerFinishScript`**: Bash script file path will be called after stream finishes.
* **`settings.webRTCFrameRate`**: It's mandatory. Determines the frame rate of video publishing to the WebRTC players. Default value is `20`
* **`settings.webrtc.portRangeMin`**: It's mandatory. Determines the minimum port number for WebRTC connection. Default value is `0`.
* **`settings.webrtc.portRangeMax`**: It's mandatory. Determines the maximum port number for WebRTC connection. Default value is `0`.
* **`settings.webrtc.stunServerURI`**: Stun server URI used for WebRTC signaling. You can check: https://antmedia.io/learn-webrtc-basics-components/. Default value is `stun:stun.l.google.com:19302`.
* **`settings.webrtc.tcpCandidateEnabled`**: It's mandatory. If it is set `true` then TCP candidates can be used for WebRTC connection. If it is false only UDP port will be used. Default value is `true`.
* **`settings.encoding.encoderName`**: Can be h264_nvenc or libx264. If you set h264_nvenc but it cannot be opened then libx264 will be used.
* **`settings.encoding.preset`**: Please check https://trac.ffmpeg.org/wiki/Encode/H.264
* **`settings.encoding.profile`**: Please check https://trac.ffmpeg.org/wiki/Encode/H.264
* **`settings.encoding.level`**: Please check https://trac.ffmpeg.org/wiki/Encode/H.264
* **`settings.encoding.rc`**: Please check https://trac.ffmpeg.org/wiki/Encode/H.264
* **`settings.encoding.specific`**: Specific settings for selected encoder. For libx264 please check https://trac.ffmpeg.org/wiki/Encode/H.264
* **`settings.defaultDecodersEnabled`**: If it is set `true`, WebRTC using default decoders(such as VP8, VP9). If it is set `false`, WebRTC using only default h264 decoder. Default value is `false`.
* **`settings.remoteAllowedCIDR`**: Allowed IP addresses to reach REST API. It must be in CIDR format as a.b.c.d/x
* **`settings.collectSocialMediaActivityEnabled`**: If it's enabled, interactivity(like, comment, etc.) is collected from social media channel. Default value is `false`.
* **`db.app.name`**: Application name such as LiveApp, WebRTCApp etc.
* **`db.name`**: Database name for the application.
* **`db.type`**: Can be mongodb or mapdb.
* **`db.host`**: Meaningful for MongoDB. It is the host address of MongoDB as `<mongo_host>:27017` or Mongo Replica Set as `<mongo_host>:27017/?replicaSet=<replication_name>`
* **`db.user`**: MongoDB user name. Left as blank if no user credentials.
* **`db.password`**: MongoDB password. Left as blank if no user credentials.
* **`webapp.dbName`**: No need this setting.
* **`webapp.contextPath`**: No need this setting.
* **`webapp.virtualHosts`**: always *.